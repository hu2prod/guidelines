# Guide для начинающего linux'овода

## Команды
### Команды которые не имеют особых опций
  * `pwd` вывести текущий каталог
### ls
Узнать список файлов и каталогов в текущей папке
  * `ls` Просмотреть список файлов в текущей директории
  * `ls -la` Просмотреть список файлов, а также скрытых файлов (которые начинаются на `.`), а также права доступа.
  * `ls *.coffee` Просмотреть спиок файлов, которые имеют расширение `.coffee`
  * `ls src` Посмотреть список файлов в директории src
  * `ls src/*.coffee` Просмотреть спиок файлов, которые имеют расширение `.coffee` в директории src
  * `ls ~` Просмотреть свою домашнюю папку
### cd
Изменить директорию.
  * `cd src` Зайти в директорию src
  * `cd ..` Зайти в директорию на 1 уровень выше
  * `cd ../src` Зайти в директорию на 1 уровень выше, а потом с этого уровня в директорию src
  * `cd -` откатить последнее изменение директории
### chmod
  * `chmod +x ./index.js` дать файлу ./index.js право на исполнение
  * `chmod +x *.js` дать всем файлам которые имеют расширение js право на исполнение
### cp, mv
cp - копирует, mv перемещает
  * `cp a b` скопировать файл `a` в `b`. Содержимое файла b будет удалено, если такой файл был
    * `mv a b`
  * `cp -r dir1 dir2` скопировать директорию `dir1` в директорию `dir2`
    * `mv dir1 dir2`
### mkdir
создать каталог
  * `mkdir my_dir` создать папку my_dir
  * `mkdir -p my_dir/my_dir2/my_dir3` создать путь my_dir/my_dir2/my_dir3, все директории которые не существуют на пути будут созданы

### ssh
зайти безопасным способом на сервер
  * `ssh root@@<188.188.188.188>` где <188.188.188.188> это IP вашего сервера, root имя пользователя на сервере

### scp
Передача файлов на сервер и с сервера\
Прим. Все целевые файлы будут или заменены, или созданы, если отсутсвуют
  * `scp local_file root@188.188.188.188:/path/on/server` скопировать local_file на сервер в папку /path/on/server
  * `scp local_file root@188.188.188.188:/path/on/server/target_file` скопировать local_file на сервер и заменить файл target_file в папке /path/on/server
  * `scp root@188.188.188.188:/path/on/server/target_file .` скопировать с сервера файл target_file в текущую папку
  * `scp root@188.188.188.188:/path/on/server/target_file local_file` скопировать с сервера файл target_file в локальный файл local_file 

## Строки интерпретаторов
  * `#!/bin/bash` интерпретатор командной строки `bash`
  * `#!/usr/env/env node` интерпретатор node.js
    * (Нужно установить [nvm](https://github.com/creationix/nvm) см. ниже)
  * `#!/usr/env/env iced` интерпретатор iced-coffee-script
    * (Нужно установить iced-coffee-script) `npm i -g iced-coffee-script`

## Рецепты. Первичная установка/настройка

### Как зайти по ssh на сервер
  * `ssh root@@<188.188.188.188>` где <188.188.188.188> это IP вашего сервера

### Как создать ключи
  * `ssh-keygen`
  * Много раз enter

### Как прописать ключи на сервере

  * У вас должны быть созданы ключи. Проверить это можно `ls ~./ssh`
  * `ssh-copy-id root@<188.188.188.188>` где <188.188.188.188> это IP вашего сервера

### Как установить nvm и первую версию node.js
  * `curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash`
  * Перелогинится в консоль
  * nvm install 6.6.0
  * node -v (перепроверить)

### Поменять пароль
  * `passwd`
  * (для root пропускаем) ввести старый пароль
  * Ввести 2 раза новый пароль

## Рецепты. Запуск и логи

  * `node ./index.js` Запустить index.js используя интерпретатор node
    * `./index.js` если ./index.js имеет право выполнения и имеет правильную первую строку, то можно и нужно так
      * `chmod +x index.js`
      * nano index.js
      * Дописываем `#!/usr/bin/env node` (также см Строки интерпретаторов)
  * `./index.js 2>&1 | tee my.log` Логи записывать в файл my.log и выводить на экран и stderr завернуть в stdout
    * Минусы. Если у вас есть цветной вывод, то он очень вероятно станет черно-белым

### Сделать процесс node самоподнимаемым
Создать файл (наприме ./start.sh) со следующим содержимым

    #!/bin/bash
    while :
    do
      echo "restart"
      ./index.js # тут что мы запускаем. Заменить на то, что нужно
      sleep 0.2
    done
  
Вспоминаем `chmod +x ./start.sh`

### Как запустить процесс, что бы он не умер после того как ты отсоединишься

  * `screen -R prod` подключиться к screen'у с именем содержащим prod, а если нету, создать такой.
    * Прим. Если кто-то уже к нему подсоединился... то создать новый
  * Запустить нужный скрипт.
  * `ctrl-A ctrl-D` выйти из screen
Как зайти обратно
  * `screen -R prod`
Прим. перезагрузка сервера убивает все screen
Вспомогательные рецепты
  * `screen -ls` список screen'ов
  * `screen -D prod` выбросить того, кто подсоединился к screen'у prod
  * `ctrl-D` просто закрыть текущий screen (только если ничего не выполняется)

## Рецепты. nano
  * `ctrl-x y enter` выйти сохранив файл
  * `ctrl-x n` выйти НЕ сохранив файл
  * `ctrl-x` выйти, если не было изменений
  * `ctrl-o enter` сохранить файл
  * `ctrl-k` удалить строку где находится курсор
