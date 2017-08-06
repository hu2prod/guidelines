# iced-coffee-script
[iced-coffee-script документация](http://maxtaco.github.io/coffee-script/)  
## Локальные особенности/договорённости
Мы используем исключительно 2 пробела. (Потому что выравнивание | в scriptscript для pipeline)  
Мы используем "умные отступы" для красивого форматирования кода. : и = должны быть по возможности выровнены через два пробела. Причём нельзя оставлять некратное количество пробелов (Текстовый редактор выравнивает по кнопке tab на кратное количество пробелов).

    a = # плохо
      prop1 : 1
      a : 2
      asdfjda : 3
      
    a = # неправильно, отсуп не кратен 2
      prop1  : 1
      a      : 2
      asdfjda: 3
      
    a = # правильно
      prop1   : 1
      a       : 2
      asdfjda : 3
    
    # плохо
    a.b = 1
    a.prop = 1
    a.adsfasdf = 1
    
    # правильно
    a.b       = 1
    a.prop    = 1
    a.adsfasdf= 1 # правильно, нет двойного пробела между идентификатором и =

В общем случае чем больше кода можно уместить на экран - тем лучше, но есть исключения.  
"Типографические отбивки", которые мы используем

    class A
      prop1 : ''
      prop2 : ''
      constructor : ()->
      # после каждой функции отступ
      go : ()->
        a = do1()
        b = do2()
        c = do3()
        a+b+c # return можно не писать
      # после каждой функции отступ
      prop1_get : ()->@prop1 # группа однострочников исключения из правил
      prop1_set : (@prop1)->
      # после них все-равно отступ
      
    # отступ между декларацией класса и любым другим кодом
    class B
    # в конце любого файла должен быть \n (иногда допустимо несколько)

multiline desctucturing assignment

    {
      a
      b
      c
    } = obj # читаемо же намного лучше и добавление нового элемента не требует запятой

delete означает метод, который удалит все ссылки

   class A
     arr : [] # может содержать ссылку на себя
     constructor : ()->
       @arr = []
     delete : ()-> # это не деструктор
       @arr.clear() # заменитель @arr.length = 0
       return # хорошим тоном является не возвращать ничего в явном виде

## Bad parts
Если ты делаешь класс, то помни про [] и {}

    class A
      # не установил в prototype - просрал производительность
      arr : []
      hash: {}
      constructor : ()->
        # не инициализировал - словил кучу багов
        @arr = []
        @hash= []

Не рекомендуется использовать

    is
    isnt
    @@a # @@ prefix operator
    a::b   # prototype operator
    on off # вместо true false
    this   # используем только @
    &&     # используем только and
    ||     # используем только or

Нельзя использовать всё то, что компилируется с дополнительными функциями. Дополнительные функции гробят производительность. Заметно. 

    cubes = (math.cube num for num in list)
    
    ages = for child, age of yearsOld # return + for 
      "#{child} is #{age}"

Нежелательно использовать map в highperf коде

    in_arr = [1,2,3]
    ret = in_arr.map((t)->t+1) # плохо
    
    # хорошо
    ret = []
    for v in in_arr
      ret.push v+1

В циклах запрещено

    eat food for food in foods when food isnt 'chocolate' # использовать when
    for k,v of [] # использовать for of для массивов. Только for in
      console.log v

Очень внимательно смотреть на пробелы

    a?b  # if (typeof a === "function") {a(b);}
    a? b # if (typeof a === "function") {a(b);}
    a ?b # if (typeof a !== "undefined" && a !== null) {a;} else {b;};
    a+b  # a + b
    a+ b # a + b
    a + b# a + b
    a +b # a(+b) # добро пожаловать на борт

В scriptscript его не будет

    a ? b

Не очевидный код

    {sculptor} = futurists # так ок
    {a,b,c} = futurists # так ок
    {
      a
      b
      c
    } = futurists # так еще больше ок, хоть и больше строк
    {poet: {name, address: [street, city]}} = futurists # да, ты мощный чувак, но нифига не понятно.

Плохой тон

    fn = (on_end)->
      res = 1
      # on_end res # даже если сейчас никакой ошибки не будет, то потом может появиться
      on_end null, res # всегда оставляй первое место для возможной ошибки

## Типичные ошибки
Забыл про то, что (почти) любое выражение в coffee-script возвращает результат

    fn = (list)->
      for v in list
        do_some v
      return # если не написать, то будет собирать результаты do_some и возвращать их массив

Забыл про [], {} в constructor

    class A
      arr : []
      hash: {}
      constructor : ()->
        @arr = [] # ты просто не представляешь насколько это частая ошибка
        @hash= {}

## Tricks
continue trick

    for v in list
      continue if condition1 v
      continue if condition2 v
      continue if condition3 v
      actual_action v

mixin use

    class Blockchain
      event_mixin @
      constructor : ()->
        event_mixin_constructor @

await + err handle in single line

    await fs.readFile file, defer(err, cont); throw err if err
    
    fn = (on_end)->
      await fs.readFile file, defer(err, cont); return on_end err if err # вариация для callback
      # если захотим что-то дописать, то сразу есть место куда, 
      on_end null, cont
    
    fn2 = (on_end)->
      await fs.readFile file, defer(err, cont); return on_end err if err
      if cont.length == 1
        return on_end new Error "Ну блин, пустой файл" # кастомные ошибки через тот же паттерн return on_end ...
      on_end null, cont

await + do combo

    await
      for file in file_list
        cb = defer()
        do (cb, file)->
          await fs.readFile file, defer(err, cont); throw err if err # настолько фатально, что внешний await пофиг
          cb()
    console.log("done")
