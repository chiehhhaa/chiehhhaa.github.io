---
title: '💡JavaScript : Function Expressions v.s. Function Statements'
date: 2024-07-08 22:17:15
code_block_shrink:  false
categories: [JavaScript]
tags: [Function, Expressions, Statements]
---

函式表達式跟函式陳述句！？這是蝦米🍤！？
對於這種定義的東西，剛開始真的是一個頭兩個大～
徹底研究一番後整理出這二位的差別，讓我們繼續看下去…
 <!-- more -->

首先，先來了解什麼是表達式？什麼又是陳述句呢？

## 表達式 (Expressions)

Expressions 就是會直接回傳一個值的一串程式，因為有回傳值所以我們可以把他存在一個變數中，但也不是一定要存啦(🤣)

舉個例子，如果直接在瀏覽器的console中輸入：

```javascript
1 + 2 
// 回傳 3

a = 5
// 回傳 5

b = {name: chiehhhaa}
//回傳 {name: chiehhhaa}
```

上面這些直接有回傳值的程式碼都是Expressions表達式。

## 陳述句 (Statements)

會把兩個放一起比較，那應該可以很明顯知道，Statements就是沒有回傳值的一串程式🍢 ！那我們也無法把他存在變數中～

來舉個例子唄：

```javascript
if(a === 18){
  console.log("Young!");
}
```

這邊的if判斷句 `a === 18` 是一個 Expressions（因為他會回傳 true 給 if 做判斷），但是，if 這整個指令是一個Statements 喔！當然我們也不能把 if 整個指令存到變數中。

> Expressions表達式：有回傳值，可以存進變數中使用。
Statements陳述句：沒有回傳值，也沒辦法存進變數中。

---

再來看看Function Expressions跟Function Statements之間有什麼差別吧！

## Function Statements 函式陳述句

```javascript
function sayHey() {
  console.log("Hey")
}

sayHey()
```

一般的函式寫法會像上面這串程式，這樣就是一種函式宣告。
前面提到 Statements 是一串沒有回傳值的程式，不過 Function Statements 有個特色是：在宣告時， `sayHey` 這個函式就會因為 Hoisting 先被儲存在記憶體中 (關於 Hoisting 可以參考：[💡JavaScript：Hoisting](https://chiehhhaa.github.io/2024/07/08/JavaScript-Hoisting/))

正因為函式宣告也有 Hoisting，所以當使用陳述句的寫法時，就可以隨時呼叫這個函式都不會出錯喲！

## Function Expressions 函式表達式

```javascript
 const sayHello = function(){
  console.log("Hello!")
}

sayHello()
```

我們已經知道 Expressions 是一串有回傳值的程式碼，並且可以將其放進一個變數中。同理！Function Expressions 也是一樣的，從上面的程式碼可以知道，我們把一個 `function(){…}` 放進一個名叫 `sayHello` 的變數中，只要呼叫 `sayHelllo()`  就能順利執行這段 function 囉！

仔細觀察有發現這段 function 有什麼特別的地方嗎？
答：它沒有函式名！
我們叫長得像這樣的函式為『匿名函式』。

再來看一段程式碼：

```javascript
sayHello()

const sayHello = function(){
  console.log("Hello!")
}
```

我們試著在宣告前呼叫這段 function，你會發現結果大噴錯💣！
錯誤訊息：
`ReferenceError: Cannot access ‘sayHello’ before initialization.`
沒錯！這個跟在使用 let/const 宣告變數時，會遇到的問題一樣，只能在確定宣告後，才能呼叫使用這串函式📢 (一樣可以參考[💡JavaScript：Hoisting](https://chiehhhaa.github.io/2024/07/08/JavaScript-Hoisting/))

如果這樣的話，那我們用 var 的話呢！

```javascript
sayHello()

var sayHello = function(){
  console.log("Hello!")
}
```

登愣！一樣會噴錯💣

錯誤訊息：
`Uncaught TypeError: sayHello is not a function`
因為 var 在宣告的時候會先給一個 undefined 值，但 undefined 並不是一個 function，所以才會出現這樣的錯誤訊息。



>👽💬：看完上面的內容，會發現 Hoisting 真的是一個很重要的觀念。把 Hoisting 的觀念與 Expressions 和 Statements 的差別結合起來去思考，應該就比較能理解它們之間的運作了。
