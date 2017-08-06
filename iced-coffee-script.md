# iced-coffee-script
[iced-coffee-script документация](http://maxtaco.github.io/coffee-script/)

## Bad parts

Нельзя использовать всё то, что компилируется с дополнительными функциями. Дополнительные функции гробят производительность. Заметно. 

    abc
    ddd

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

