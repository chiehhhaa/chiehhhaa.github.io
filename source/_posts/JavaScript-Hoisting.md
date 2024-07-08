---
title: 'JavaScript : Hoisting'
date: 2024-07-08 19:02:46
code_block_shrink:  false
categories: [JavaScript]
tags: [Hoisting]
---

# 💡JavaScript : Hoisting

如果有任何錯誤的地方，還請各位指教～🥺🙏🏼

第一篇獻給我認為很重要也需要花時間好好瞭解的觀念：Hoisting 🤯

👀 先看一段程式碼：
```javascript
console.log(a);
b();

var a = "Hello!"; 
function b() {
  console.log("World")
}
```

使用變數時，一定要先宣告！這是很重要的，但依照上面這段程式碼的結果，照理來說會噴錯，但！這邊竟然沒有紅字產生！

得到的結果會是：
```javascript
// undefined
// World
```

Why!?🧐

這就是這篇筆記的重點了— **Hoisting**！

在 [w3schools](https://www.w3schools.com/js/js_hoisting.asp) 中所描述的 hoisting：
> Hoisting is JavaScript’s default behavior of moving declarations to the top.

意思是在JavaScript中，它會把定義的變項移到最前面先執行。所以上面的程式碼才沒有出現錯誤。

在一開始執行 `var a` 和 `function b` 的時候，JS就已經將這兩個名字儲存在記憶體中，只是剛開始的 `a` 還沒賦值，所以先給他undefined這個特殊值，這就是為什麼結果會是 `undefined` 和 `World` 。

🧑🏻‍🏫 上課時，老師有提到：
> 宣告變數時有兩個時期 1. 建立期 2. 執行期

var 在建立期時除了給名字以外，還會有一個初始化的動作（註：初始化就是先給變數一個初始值，以上面的例子來說，a 的初始值就是undefined ）
執行期時如果需要給新值就會有一個賦值的動作，最後就是執行程式碼了。

補充在 [w3schools](https://www.w3schools.com/js/js_hoisting.asp) 中的説明：

>JavaScript Declarations are Hoisted.
JavaScript Initializations are Not Hoisted.

JS中宣告會提升，而初始化不會提升。

`var a = “Hello!”`，只有宣告 a 這件事會被提升到作用域的最前頭，而 ”Hello!” 這個初始值不會一起提升，所以才會出現`undefined`。

---

再來看看第二段程式碼：
```javascript
console.log(c);
let c = "Hola!"
```

得到的結果會是：

```javascript
// ReferenceError: Cannot access 'c' before initialization
```

奇怪，依照上面所講的，為什麼這邊會噴一個 Error 呢？
仔細看內容：無法在 c 初始化前使用。
咦？Why!?🤨

這邊就是這篇的第二個重點，關於 let/const 的 Hoisting !

let/const 的提升也會分為兩個階段 1. 建立期 2. 執行期，但與 var 不同的地方是，在建立期的時候：一樣會給名，但這邊的變數並不會初始化，而是進入一個暫時性死區 `TDZ（Temporal Dead Zone)` ，在執行的階段因為變數沒有初始值，所以才會噴出上面的錯誤訊息。

由此可知，如果宣告變數是使用 let/const 的話，就一定要在確定宣告後再使用。
