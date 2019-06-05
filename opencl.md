# opencl performance guide
Не знаешь какой вариант выбрать, выбирай жирный

## worksize (local)
Прим. для карт amd
 * 64 почти всегда плохо. -30% -- -50% производительности, но стоит один раз попробовать
 * **128** самый лучший вариант в среднем.
 * 256 обычно не дает преимущества, но стоит попрробывать

## циклы
 * full unroll +5% perf, но в некоторых случаях при очень больших объемах кода просто не стартует
 * **1 for вполне ок**, amd opencl компилятор нормально оптимизирует такие циклы, но желательно в цикле иметь что-то тяжелое
   * доступ к памяти
   * много инструкций
 * Несколько вложенных циклов. Почти всегда плохо, но иногда без этого нельзя.
   * если есть возможность раскрыть внутренний цикл - неплохо

## if vs ternary vs mem access
 * тернарки всегда лучше if'ов на вычислениях
 * если доступ к памяти в одну и ту же ячейку со всех потоков. if плохо
 * если доступ к памяти с в разные ячейки if обычно полезен, но надо мерять

## local и barrier
 * *не уверен - не используй*
 * загрузить 1 раз в local, оградиться barrier(CLK_LOCAL_MEM_FENCE) и использовать много раз неплохой паттерн
   * [пример 1](https://github.com/brian112358/avermore-miner/blob/43e36dff5c21ad1ba98ef4b0efa07592780028d8/kernel/x16.cl#L348)
 * *не уверен - не используй* barrier(CLK_GLOBAL_MEM_FENCE)
   * Даже если уверен - не используй
   * Имеет смысл разбивать на 2 kernel'а

## способы работы с памятью
 * _straightforward pipeline strict_
   * host -> readonly buffer
   * readonly buffer -> kernel -> writeonly buffer
   * writeonly buffer -> host
 * _straightforward pipeline_
   * host -> readonly buffer
   * readonly buffer -> kernel -> read/write buffer
   * read/write buffer -> kernel -> writeonly buffer
   * writeonly buffer -> host
   * Прим. нежелательно читать и писать в один и тот же буффер одним и тем же kernel'ом
 * straightforward pipeline + dma trick
   * stage 1 host -> readonly buffer 1
   * stage 2
     * host -> readonly buffer 2
     * readonly buffer 1 -> kernel -> writeonly buffer 1
   * stage 3
     * host -> readonly buffer 1
     * readonly buffer 2 -> kernel -> writeonly buffer 2
     * writeonly buffer 1 -> host
## Идеальные заготовки
### 1 in - 1 out
 * low compute
   * vectorized read
   * vectorized write
   * ~10000 потоков (обычно x2-x3 от количества CU в видеокарте)
 * high compute
   * точно должны использоваться все CU
     * т.е. совет ~10000 потоков имеет первый приоритет
   * см. рецепты low compute
### N in - 1 out
 * обычно в таком случае несколько входов надодятся рядом
 * vectorized read
 * vertorized write
 * хорошие соотношения количества r/w
   * 8 r 4 w
   * 4+N r 4 w
 * если значение уже прочитали, то не надо его еще раз перечитывать с памяти
   * проще его прочитать из локальной переменной
   * Пример. Для подсчета res1 мы считали N=3
     * пускай в массиве glob хранятся значения в памяти
     * x0 = glob[0]
     * x1 = glob[1]
     * x2 = glob[2]
     * Для следующего элемента нам надо glob[1] glob[2] glob[3]
     * Медленно
       * x0 = glob[1]
       * x1 = glob[2]
       * x2 = glob[3]
     * Лучше
       * x0 = x1
       * x1 = x2
       * x2 = glob[3]
     * Еще лучше
       * изначально считывать еще больше x0 - x7
       * посчитать y1 - y6
       * x0 = x6
       * x1 = x7
       * для x2 - x7 читаем с glob