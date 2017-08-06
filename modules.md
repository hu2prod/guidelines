must known API

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
 * https://www.npmjs.com/package/serve-static (**не всегда рекомендуется, есть по-лучше альтернатива**)
 * https://www.npmjs.com/package/compression

Модули для
 * mongodb https://www.npmjs.com/package/mongodb
 * postgres https://www.npmjs.com/package/pg (**давно не использовал**)

Fix me

 * fix js https://github.com/hu2prod/fy
 * event_mixin
 * lock_mixin
 * Websocket_wrap

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

Сделать бочку

 * Arduino https://www.npmjs.com/package/johnny-five
 * OpenCL https://www.npmjs.com/package/nooocl (Но лучше юзать обёртку gpu см distrib_gpu)

Прочие решения

 * На чём писать GUI? Покачто на React
