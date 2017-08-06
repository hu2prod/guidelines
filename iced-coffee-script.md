# iced-coffee-script
[iced-coffee-script документация](http://maxtaco.github.io/coffee-script/)  
## Локальные особенности
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

await + do combo

    await
      for file in file_list
        cb = defer()
        do (cb, file)->
          await fs.readFile file, defer(err, cont); throw err if err
          cb()
    console.log("done")
