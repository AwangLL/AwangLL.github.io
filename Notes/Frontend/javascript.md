# javasript

## js导入方式

### 内部Js

```html
<script>
// Code
</script>
```

### 外部Js

```html
<script src="script.js" async></script>
```

>**注**：“外部”示例中 `async` 属性可以解决调用顺序问题，因此无需使用 `DOMContentLoaded` 事件。而 `async` 只能用于外部脚本，因此不适用于“内部”示例。

### 脚本调用策略

问题：HTML 元素是按其在页面中出现的次序调用的，如果用 JavaScript 来管理页面上的元素（更精确的说法是使用文档对象模型DOM），若 JavaScript 加载于欲操作的 HTML 元素之前，则代码将出错。

可利用`sync`或`defer`来解决上述问题。

例如要加载以下三个脚本

```html
<script async src="js/vendor/jquery.js"></script>

<script async src="js/script2.js"></script>

<script async src="js/script3.js"></script>
```

三者的调用顺序是不确定的。`jquery.js` 可能在 `script2` 和 `script3` 之前或之后调用，如果这样，后两个脚本中依赖 `jquery` 的函数将产生错误，因为脚本运行时 `jquery` 尚未加载。

解决这一问题可使用 `defer` 属性，脚本将按照在页面中出现的顺序加载和运行：

```html
<script defer src="js/vendor/jquery.js"></script>

<script defer src="js/script2.js"></script>

<script defer src="js/script3.js"></script>
```

添加 `defer` 属性的脚本将按照在页面中出现的顺序加载，因此第二个示例可确保 `jquery.js` 必定加载于 `script2.js` 和 `script3.js` 之前，同时 `script2.js` 必定加载于 `script3.js` 之前。

脚本调用策略小结：

-   如果脚本无需等待页面解析，且无依赖独立运行，那么应使用 `async`。
-   如果脚本需要等待页面解析，且依赖于其它脚本，调用这些脚本时应使用 `defer`，将关联的脚本按所需顺序置于 HTML 中。

## 注释

```js
// 我是单号注释

/*
	我是
	多行
	注释
/
```

## 变量

### 声明变量

```js
let myName;
var myAge;
```

> `var`与`let`区别
> 当你使用 `var` 时，可以根据需要多次声明相同名称的变量，但是 `let` 不能。
> 可以在初始化一个变量之后用 `var` 声明它，但是 `let` 不能。

### 初始化变量

```js
myName = 'jack';
var myAge = 10;
```

### 更新变量

```js
myName = 'Bob';
myAge = 40;
```

### 变量类型

- `Number`数字类型，可以是整数和小数
```js
let myAge = 18;
```
- `String`字符串
```js
let hello = "hello world";
```
- `Boolean`布尔类型，只有`true`和`false`两个值
```js
let flag = true;
```
- `Array`数组是单个对象，其中包含很多值，方括号括起来，并用逗号分隔。
```js
// 声明
let classmate = ['jack', 'tom', 'john'];
// 检索
classmate[1];
```
- `Object`对象
```js
// 声明
let dog = {name: 'Spot', age: 2};
// 检索
dog.name;
```

### 动态类型

JavaScript 是一种“动态类型语言”，不需要指定变量将包含什么数据类型。

```js
let myNumber = '500'; // oops, this is still a string
typeof myNumber;
myNumber = 500; // much better — now this is a number
typeof myNumber
```

> `typeof`操作符：返回所传递给它的变量的数据类型。

## 函数

### 函数声明

```js
function functionName() {

}
```

## 运算符

### 算术运算符

```js
+
-
*
/
%
**
++
--
```

### 赋值运算符

```js
+=
-=
*=
/=
```

### 比较运算符

```js
===
!==
<
>
<=
>=
```

## 条件语句

```js
if(expression) {
	
}
else {

}

if(expression) {
	
} else if(expression) {

} else if(expression) {

} else {

}
```

## 循环

### for

```js
for(let i  = 1; i < 10; i++) {
	console.log(i);
}
```

## 异步JavaScript

异步编程技术使你的程序可以在执行一个可能长期运行的任务的同时继续对其他事件做出反应而不必等待任务完成。与此同时，你的程序也将在任务完成后显示结果。

浏览器提供的许多功能可能需要很长的时间来完成，因此需要异步完成，例如：

- 使用`fetch()`发起HTTP请求
- 使用`getUserMedia()`访问用户的摄像头和麦克风
- 使用`showOpenFilePicker()`请求用户选择文件以供访问

### 同步编程

浏览器按照我们书写代码的顺序一行一行地执行程序。

当耗时较久的同步函数运行时，用户不能做其他任何事情直到同步函数执行完毕。

这就是耗时的同步函数的基本问题。在这里我们想要的是一种方法，以让我们的程序可以：

-   通过调用一个函数来启动一个长期运行的操作
-   让函数开始操作并立即返回，这样我们的程序就可以保持对其他事件做出反应的能力
-   当操作最终完成时，通知我们操作的结果。

### 事件处理程序

我们在添加了事件监听器后发送请求。注意，在这之后，我们仍然可以在控制台中输出“请求已发起”，也就是说，我们的程序可以在请求进行的同时继续运行，而我们的事件处理程序将在请求完成时被调用。

```html
<button id="xhr">点击发起请求</button>
<button id="reload">重载</button>

<pre readonly class="event-log"></pre>
```

```js
const log = document.querySelector('.event-log');
document.querySelector('#xhr').addEventListener('click', () => {
  log.textContent = '';
  const xhr = new XMLHttpRequest();
  xhr.addEventListener('loadend', () => {
    log.textContent = `${log.textContent}完成！状态码：${xhr.status}`;
  });
  xhr.open('GET', 'https://raw.githubusercontent.com/mdn/content/main/files/en-us/_wikihistory.json');
  xhr.send();
  log.textContent = `${log.textContent}请求已发起\n`;});
document.querySelector('#reload').addEventListener('click', () => {
  log.textContent = '';
  document.location.reload();
});
```

由于必须在回调函数中调用回调函数，导致深度嵌套的函数更难阅读和调试，所以大多数现代异步API都不使用回调

### Promise

**Promise** 是现代 JavaScript 中异步编程的基础，是一个由异步函数返回的可以向我们指示当前操作所处的状态的对象。在 Promise 返回给调用者的时候，操作往往还没有完成，但 Promise 对象可以让我们操作最终完成时对其进行处理（无论成功还是失败）。

在基于 Promise 的 API 中，异步函数会启动操作并返回 `Promise`对象。然后，你可以将处理函数附加到 Promise 对象上，当操作完成时（成功或失败），这些处理函数将被执行。

### fetch() API

一个现代的、基于Promise的、用于替代`XMLHttpRequest`的方法。

```js
const fetchPromise = fetch('https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json');

console.log(fetchPromise);

fetchPromise.then( response => {
  console.log(`已收到响应：${response.status}`);
});

console.log("已发送请求……");
```

1. 调用`fetch()`API，并将返回值赋给`fetchPromise`变量。
2. 输出`fetchPromise`变量，输出结果应该像这样：`Promise { <state>: "pending" }`。这告诉我们有一个 `Promise` 对象，它有一个 `state`属性，值是 `"pending"`。`"pending"` 状态意味着操作仍在进行中。
3. 将一个处理函数传递给 Promise 的 `then()` 方法。当（如果）获取操作成功时，Promise 将调用我们的处理函数，传入一个包含服务器的响应的`Response`对象。
4. 输出一条信息，说明我们已经发送了这个请求。

### 链式使用Promise

在你通过 `fetch()` API 得到一个 `Response` 对象的时候，你需要调用另一个函数来获取响应数据。这次，我们想获得JSON格式的响应数据，所以我们会调用 `Response` 对象的 `json()` 方法。事实上，`json()` 也是异步的，因此我们必须连续调用两个异步函数。

```js
const fetchPromise = fetch('https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json');

fetchPromise.then( response => {
  const jsonPromise = response.json();
  jsonPromise.then( json => {
    console.log(json[0].name);
  });
});
```

在这个示例中，给 `fetch()` 返回的 Promise 对象添加了一个 `then()` 处理程序。但这次我们的处理程序调用 `response.json()` 方法，然后将一个新的 `then()` 处理程序传递到 `response.json()` 返回的 Promise 中。

使用Promise与之前利用回调函数的区别是`then()`本身也会返回一个Promise，这个Promise将指示`then()`中调用的异步函数的完成状态，也就是说可以把代码改成：

```js
fetchPromise
  .then( response => {
    return response.json();
  })
  .then( json => {
    console.log(json[0].name);
  });
```

>我们需要在尝试读取请求之前检查服务器是否接受并处理了该请求。我们将通过检查响应中的状态码来做到这一点，如果状态码不是“OK”，就抛出一个错误：

```js
fetchPromise
  .then( response => {
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    return response.json();
  })
  .then( json => {
    console.log(json[0].name);
  });
```

### 错误捕获

`Promise` 对象提供了一个 `catch()`方法来支持错误处理。然后，当异步操作成功时，传递给 `then()` 的处理函数被调用，而当异步操作_失败_时，传递给 `catch()` 的处理函数被调用。

如果将 `catch()` 添加到 Promise 链的末尾，它就可以在任何异步函数失败时被调用。

```js
const fetchPromise = fetch('bad-scheme://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json');

fetchPromise
  .then( response => {
    if (!response.ok) {
      throw new Error(`HTTP 请求错误：${response.status}`);
    }
    return response.json();
  })
  .then( json => {
    console.log(json[0].name);
  })
  .catch( error => {
    console.error(`无法获取产品列表：${error}`);
  });

```

### Promise术语

Promise的三种状态：

- **待定(pending)**：初始状态，既没有被兑现，也没有被拒绝。这是调用 `fetch()` 返回 Promise 时的状态，此时请求还在进行中。
- **已兑现(fulfilled)**：意味着操作成功完成。当 Promise 完成时，它的 `then()` 处理函数被调用。
- **已拒绝(rejected)**：意味着操作失败。当一个 Promise 失败时，它的 `catch()` 处理函数被调用。

> 这里的“成功”或“失败”的含义取决于所使用的 API：例如，`fetch()` 认为服务器返回一个错误（如404 Not Found时请求成功，但如果网络错误阻止请求被发送，则认为请求失败。

### 合并多个Promise

当需要所有的Promise都得到实现，但他们并不互相依赖，需要借助`Promise.all()`方法，它接收一个Promise数组并返回一个单一的Promise。

由`Promise.all()`返回的Promise：

- 当且仅当数组中所有的Promise都被兑现时，才会通知`then()` 处理函数并提供一个包含所有响应的数组，数组中响应的顺序与被传入 `all()` 的 Promise 的顺序相同。
- 会被拒绝——如果数组中有**任何一个**Promise 被拒绝。此时，`catch()` 处理函数被调用，并提供被拒绝的 Promise 所抛出的错误。

有时，可能需要一组Promise中的某一个Promise实现，而不关心是哪一个。在这种情况下，需要用到`Promise.any()`方法。在 Promise 数组中的任何一个被兑现时它就会被兑现，如果所有的 Promise 都被拒绝，它也会被拒绝。

### async 和 await

`async` 关键字为你提供了一种更简单的方法来处理基于异步 Promise 的代码。

在异步函数中，你可以在调用一个返回 Promise 的函数之前使用 `await` 关键字。这使得代码在该点上等待，直到 Promise 被完成，这时 Promise 的响应被当作返回值，或者被拒绝的响应被作为错误抛出。

```js
async function fetchProducts() {
  try {
    // 在这一行之后，我们的函数将等待 `fetch()` 调用完成
    // 调用 `fetch()` 将返回一个“响应”或抛出一个错误
    const response = await fetch('https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json');
    if (!response.ok) {
      throw new Error(`HTTP 请求错误：${response.status}`);
    }
    // 在这一行之后，我们的函数将等待 `response.json()` 的调用完成
    // `response.json()` 调用将返回 JSON 对象或抛出一个错误
    const json = await response.json();
    console.log(json[0].name);
  }
  catch(error) {
    console.error(`无法获取产品列表：${error}`);
  }
}

fetchProducts();
```

这里我们调用 `await fetch()`，我们的调用者得到的并不是 `Promise`，而是一个完整的 `Response` 对象，就好像 `fetch()` 是一个同步函数一样。

但这个写法只在异步函数中起作用。异步函数总是返回一个 Pomise，所以你不能做这样的事情：

```js
async function fetchProducts() {
  try {
    const response = await fetch('https://mdn.github.io/learning-area/javascript/apis/fetching-data/can-store/products.json');
    if (!response.ok) {
      throw new Error(`HTTP 请求错误：${response.status}`);
    }
    const json = await response.json();
    return json;
  }
  catch(error) {
    console.error(`无法获取产品列表：${error}`);
  }
}


/* 错误 */
const json = fetchProducts();
console.log(json[0].name);   // json 是一个 Promise 对象，因此这句代码无法正常工作

/* 正确 */
const jsonPromise = fetchProducts();
jsonPromise.then((json) => console.log(json[0].name));

```

## 实现基于 Promise 的 API
