# 目錄
- [第一章 零基礎關鍵灌輸篇](#第一章-零基礎灌輸篇)
- [第二章 開發好用工具篇](#第二章-開發好用工具篇)
- [第三章 javascript新手須知篇](#第三章-javascript新手須知篇)
- [第四章 邏輯判斷篇](#第四章-邏輯判斷篇)
    - [判斷式 if](#判斷式-if)
    - [判斷式 switch](#判斷式-switch)
- [第五章 DOM物件與事件篇](#第五章-DOM物件與事件篇)
    - [獲取DOM物件](#獲取DOM物件)
    - [onclick事件](#onclick事件)
    - [操作DOM屬性](#操作DOM屬性)
    - [操作DOM style](#操作DOM-style)    
    - [匿名涵式](#匿名涵式)
    - [事件綁定兩種寫法](#事件綁定兩種寫法)
    - [innerHTML & innerText](#innerhtml--innertext)
    - [常用的window方法](#常用的window方法)
    - [onmouseover & onmouseout](#onmouseover--onmouseout)
    - [照片牆展示](#照片牆展示)
- [第六章 迴圈入門篇](#第六章-迴圈入門篇)
    - [while迴圈](#while迴圈)
    - [for迴圈](#for迴圈)
    - [全域變數與區域變數](#全域變數與區域變數)
- [第七章 複雜資料庫陣列入門篇](#第七章-複雜資料庫陣列入門篇)
    - [一般資料類型和複雜資料類型](#一般資料類型和複雜資料類型)
    - [陣列](#陣列)
- [第八章 陳列迴圈組合篇](#第八章-陳列迴圈組合篇)
- [第九章 class的應用](#第九章-class的應用)
- [第十章 時間計時器](#第十章-時間計時器)
    -[計時器](#計時器)
    -[照片牆自動撥放](#照片牆自動撥放)
- [第十一章 JQuery入門篇](#第十一章-JQuery入門篇)
    -[計時器](#計時器)
    -[照片牆自動撥放](#照片牆自動撥放)  
- [第十二章 課堂總結](#第十二章-課堂總結)
- [第十三章 課後輔導](#第十三章-課後輔導)      
    -[hoisting](#hoisting)
    -[MATH函數](#MATH函數)
    -[小數點陷阱](#小數點陷阱)

---

# 第一章 零基礎關鍵灌輸篇

### 前端程式語言分成三個:  HTML,CSS,javascript

**W3C: 統合網路平台技術不相同的問題**

# HTML、CSS 和 JavaScript 的功能對比  

| **項目**      | **類比**          | **功能描述**                          | **是否能直接理解**         |  
|---------------|-------------------|---------------------------------------|----------------------------|  
| **HTML**      | 骨架              | 定義網頁的結構與內容，例如文字、圖像等 | 原始碼（可直接知道意思）    |  
| **CSS**       | 外觀              | 設定網頁的樣式，例如顏色、字體、排版  | 原始碼（可直接知道意思）    |  
| **JavaScript**| 大腦              | 為網頁增加交互性與邏輯處理，例如按鈕點擊、數據處理等 | 程式（無法直接知道意思，需要執行後觀察效果） |  



**HTML**

```HTML
<h1>這是原始碼</h1>
```

**CSS**

```CSS
h1{
    font-size:20px
}
```
  
**javascript**

```js
var n=1;
if(n==1){
    console.log('這是程式');
}
```

### jQuery vs javascript

> jQuery 是一種 javascript 的涵式庫，基本上 javascript能做到  jQuery 也能做到


### ESMAScript

**現在通用版本:ES5(2009本版)**

> 如果用不同的版本，要小心瀏覽器不支援的問題

--- 

# 第二章 開發好用工具篇

### vscode

1. 先下載VScode

2. 在VScode裡安裝下列套件:
    - chinese
    - Monokai Dark Soda
    - Live Server 
    - AutoFileName
    - vscode-icons
    - Copy filename

### Chorme 開發著工具

- F12

- 滑鼠右鍵 -> 檢查

# 第三章 javascript新手須知篇

### alert介紹

> 在JS裡面最後尾巴式()結尾的話為執行命令

```js
alert(0);
```
> alert會把所有執行續卡住，一定要按確定才會往下走 

> 程式碼是由上往下執行

```js
alert(0);
alert(1);
alert(2);
alert(3);
```

結果: 0 -> 1 -> 2 ->3

### DOM的載入順序和onload事件

**請不要寫在同一個HTML檔案裏面!!應該把.JS檔案獨立出來並且使用引入的方式**

```html
<script scr="./js-path.js"></script>
```

> script 一定要擺在下面，因為要等DOM元素載完才呼叫JS，不然程式會壞掉

解決方法:把程式碼放入windows.onload裡面

```js
//等畫面元素下載完成後才執行，例如圖片
windows.onload = function()(
    alert(1);
)
```

> 可在開發者工具的Network查看是否有效!

### Debug Console.log()

```js
windows.onload = function()(
   console.log("我到畫面上了");
)
```

> 訊息在開發者工具的console面板查看

```js
windows.onload = function()(
   console.error("發生錯誤了");
)
```

> 訊息在開發者工具的console面板查看(有紅色錯誤的警戒)

> 其他方法可在gooogle打 js console查看

### 變數和保留字

**宣告變數**

```js
windows.onload = function()(
  var a = 1;
  console.log(a);
)
```
> a為1

**保留字**
- 不能拿javascript以有的api命名
- 不能命名為中文
- 不能數字在前面
- 大小寫是不同的東西

**變數類型**

```js
// 三種基本型別： 布林(Boolean)、數值(Number)、字串(String)
var number = 1;
var string = "HISKIO";
var boolean = true;
        
// 兩種複合的型別：陣列(Array)、物件(Object)
var array = [];
var object = {};
        
// 兩種簡單型別：空值(null)、未定義(undefined)
var Null = null;
var Undefined;

// 特殊型別：函式(Function)
var fu = function(){};


// 不可以命名變數用原本js的api當成變數，大小寫有分
// var null = "123";
```

**變數運算**

```js
 var num1 = 1 + 1;
 console.log(num1); //2
 var num2 = 5 - 2;   
 console.log(num2); //3
 var num3 = 2 * 2;
 console.log(num3); //4
 var num4 = 10 / 3;
 console.log(num4); //3.3333...

 // 特殊運算子轉型
 var test1 = "5" + 1;
 console.log(test1); //51
 var test2 = "6" - 1;
 console.log(test2); //5
 var test3 = 1 + "2";
 console.log(test3); //12
 var test4 = 5 - "2";
 console.log(test4); //3
```

> 要小心變數類別，字串和數字相加會有問題!

--- 

# 第四章 邏輯判斷篇

## 判斷式 if

**等於的意義**

- 一個等於是指派(assign)

```js
 var weather = "下雨";
```

> 把下雨指派到weather這個變數裡面

- 兩個等於是判斷值是否相同

```js
 var weather = "下雨";
 console.log(weather == "下雨")
```
>  當weather為下雨時會console出true，反之為false


- 三個等於不只判斷值是否相同，也判斷類型是否相同

```js
 var num = 1;
 console.log(num === "1") //false
 console.log(num == "1") //true
```

=> 為了嚴謹一點，建議都寫三個等於

**if用法**

```js
 var weather = "下雨";

 if(weather == "下雨"){
    alert("今天要帶傘");
 }
```

**if else用法**

```js
 var weather = "下雨";
 if(weather == "下雨"){
    alert("今天要帶傘");
 }else{
    alert("今天不要帶傘");
}
```

**多個if else用法**

```js
 var breakfast = "麥香雞肉堡";

if(breakfast == "麥香雞肉堡"){
    alert("收你30元");
}else if(breakfast == "厚切豬排堡"){
    alert("收你85元");
}else if(breakfast == "總匯蛋餅"){
    alert("收你50元");
}else if(breakfast == "玉米濃湯"){
    alert("收你35元");
}else if(breakfast == "捷克厚牛芝加哥堡"){
    alert("收你75元");
}else{
    alert("其它一律收你55元");
}

```



## 判斷式 switch