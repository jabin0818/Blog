`promisify` 是 Node.js 中的一个工具函数，用于将基于回调的异步函数转换为返回 Promise 的异步函数。这样，你可以更方便地使用 Promise 的语法处理异步操作，而不必依赖回调函数。

在 Node.js 中，很多核心模块和第三方库的 API 使用回调函数来处理异步操作。而使用 Promise 可以使异步代码更具可读性和易维护性。`promisify` 函数可以将具有回调风格的函数转换为 Promise。

下面是一个简单的例子，假设有一个使用回调的异步函数：

```
javascriptCopy code
const fs = require('fs');

function readFileCallback(path, callback) {
  fs.readFile(path, 'utf8', (err, data) => {
    if (err) {
      callback(err, null);
    } else {
      callback(null, data);
    }
  });
}

```

你可以使用 `promisify` 转换它为返回 Promise 的函数：

```
javascriptCopy code
const util = require('util');
const readFilePromise = util.promisify(readFileCallback);

// 使用返回 Promise 的函数
readFilePromise('example.txt')
  .then(data => {
    console.log(data);
  })
  .catch(err => {
    console.error(err);
  });

```

在这个例子中，`util.promisify` 将 `readFileCallback` 函数转换为返回 Promise 的函数 `readFilePromise`。这使得你可以使用 Promise 的 `.then` 和 `.catch` 方法处理异步操作，而不必传递回调函数。



### Node 环境中的事件环（Event Loop)