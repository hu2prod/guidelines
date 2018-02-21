must know API

 * https://nodejs.org/api/http.html
 * https://nodejs.org/api/https.html
 * https://nodejs.org/api/fs.html
 * https://nodejs.org/api/buffer.html
 * https://nodejs.org/api/url.html
 * https://nodejs.org/api/events.html (прочитать и больше **НИКОГДА** так не делать. Использовать event_mixin)
 
Express и компания

 * https://www.npmjs.com/package/express
 * https://www.npmjs.com/package/cookie-parser
 * https://www.npmjs.com/package/body-parser
 * https://www.npmjs.com/package/serve-static (**не всегда рекомендуется, есть по-лучше альтернатива (cache)**)
 * https://www.npmjs.com/package/compression

Модули для
 * mongodb https://www.npmjs.com/package/mongodb
 * postgres https://www.npmjs.com/package/pg (**давно не использовал**)
   * TODO попробовать https://github.com/mscdex/pg2

Fix me

 * fix js https://github.com/hu2prod/fy
 * fix EventEmitter https://github.com/hu2prod/event_mixin
   * Также см https://github.com/hu2prod/lock_mixin как еще один полезный mixin
 * fix websocket reconnect https://github.com/hu2prod/ws_wrap

Полезные тулзы

 * Вместо es6    https://www.npmjs.com/package/iced-coffee-script
 * Вместо packer https://www.npmjs.com/package/google-closure-compiler
 * Сжимаем css   https://www.npmjs.com/package/yuicompressor
 * Вместо css    https://www.npmjs.com/package/stylus (не рекомендуются less, sass и другие)
 * Вместо curl   https://www.npmjs.com/package/request
 * Вместо jquery (serverside) https://www.npmjs.com/package/cheerio
 * Вместо fs.watch https://www.npmjs.com/package/chokidar
 * Вместо bash   https://www.npmjs.com/package/shelljs
 * Вместо getopt https://www.npmjs.com/package/minimist
 * Вместо Date.now если нужно мерять performance https://www.npmjs.com/package/performance-now
 * Если нужно зафиксировать random https://www.npmjs.com/package/random-seed

Веб сокеты

 * быстро и в прод https://www.npmjs.com/package/ws
 * high perf, но не всё поддерживает https://www.npmjs.com/package/uws
 * Не использовать (socket.io, primus)

Тесты

 * https://www.npmjs.com/package/mocha
 * https://www.npmjs.com/package/iced-coffee-coverage
 * https://www.npmjs.com/package/phantomflow Если надо тестировать web-gui. **Suite'ы выполняются параллельно.**
   * TODO такое же решение на jsdom

Сделать бочку

 * Arduino https://www.npmjs.com/package/johnny-five
   * Для кастомных прошивок https://www.npmjs.com/package/serialport (были претензии к реализации, может зависать, делал патчи. **Нужно перепроверить**)
 * OpenCL https://www.npmjs.com/package/nooocl (Но лучше юзать обёртку gpu см distrib_gpu)
 * Веб-камера https://www.npmjs.com/package/v4l2camera

Прочие решения

 * На чём писать GUI? Пока на React.
 * 3d в браузере? Threejs.
 * Использовать ли webpack? Нет, лучше webcom. Лучше нужное допилить.
 * Как отлаживать? --inspect + браузер chrome
   * Не забываем менять localhost в строке на IP удалённого сервака
   * Еще есть ключ --debug-brk для полной остановки процесса на старте, продолжить можно в отладчике в браузере

Не определился

 * Лучшая библиотека для длинной арифметики
   * https://www.npmjs.com/package/bignumber.js (кроссплатформенность)
   * https://www.npmjs.com/package/bigint (libgmp, производительность))
 * Лучшая библиотека для криптографии
   * https://www.npmjs.com/package/cryptojs (много всего, кроссплатформенность)
   * https://nodejs.org/api/crypto.html (встроено, производительность)
   * openssl (для странных вещей, не определился с модулем, потому exec)
 * Лучшая библиотека для обработки изображений
   * https://www.npmjs.com/package/canvas (почти полный canvas API на serverside)
   * https://github.com/y-a-v-a/node-gd (GD, **этот модуль не пробовал**)
   * https://www.npmjs.com/package/magick (image magick, хорош для очень больших изображений, есть рецепты, **этот модуль не пробовал**)
     * ultimate tutorial с кучей примеров http://www.imagemagick.org/Usage/
   * Сырое чтение и запись
     * https://www.npmjs.com/package/jpeg-turbo
     * https://www.npmjs.com/package/pngjs 
 * Лучшая библиотека для работы с xml
   * Лучшее решение - уйти от работодателя, который заставляет тебя работать с xml
   * https://www.npmjs.com/package/sax (**этот модуль не пробовал**)
   * https://www.npmjs.com/package/xmldom (**этот модуль не пробовал**)
 * Лучшая библиотека для общения между потоками
   * https://www.npmjs.com/package/ws (+ wrapper на reconnect)
   * https://github.com/squaremo/rabbit.js (минус rabbitmq имеет достаточно внезапный предел message per second)
     * Документация http://www.squaremobius.net/rabbit.js/
   * IPC ИМХО работает плохо. RAW TCP пока не умею готовить.
 * Лучшая библиотека для email
   * https://www.npmjs.com/package/imap (**этот модуль не пробовал**)
     * Заводишь email на gmail и шлешь письма с него через imap.
     * Не надо сетапить свой сервер (а даже если такой есть, то на него можно зайти по imap)
     * gmail первые письма отправляет +- нормально и спам фильтр их пропускает.
     * Можно читать письма, а не просто отправили и ничего не можем получить в ответ.
 * Лучшая библиотека для ssh
   * shelljs + exec ssh и поехали. В большинстве случаев это решает проблему максимально быстро
   * https://github.com/mscdex/ssh2 (**этот модуль не пробовал**)
 * Лучшая библиотека для pdf
   * https://www.npmjs.com/package/pdf.js (**этот модуль не пробовал**)
   * https://www.npmjs.com/package/canvas (пишут, что может pdf, **этот модуль не пробовал для pdf**)
 * Лучшая библиотека для работы с офисными документами (Excel, word)
   * https://www.npmjs.com/package/xlsx (**мало использовал**)
 * Лучшая библиотека для работы с разными кодировками
   * Лучшее решение - организационное. Использовать только самую популярную кодировку (UTF-8). Прим. самая популярная != самая наилучшая. Мне UTF-16 нравится больше.
   * https://www.npmjs.com/package/iconv (**этот модуль не пробовал**)

Experimental queue

  * Замена phantomjs https://github.com/graphcool/chromeless
