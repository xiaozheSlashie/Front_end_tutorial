# 目錄

- [第一章 複雜資料進階篇](#第一章-複雜資料進階篇)
  - [物件](#物件)
  - [陣列](#陣列)
  - [function 回傳](#function-回傳)
  - [操作資料](#操作資料)
- [第二章 ](#第二章-進階字串處理)
- [第三章 非同步處理](#第三章-非同步處理)
  - [介紹 api](#課程-api)
  - [非同步介紹](#非同步介紹)
  - [Json 與 FormData](#Json-與-FormData)
  - [非同步資料動態渲染列表時做](#非同步資料動態渲染列表時做)
  - [常見的 HTTP 狀態碼](#常見的-HTTP-狀態碼)
  - [跨網域存取 CROS](#跨網域存取-CROS)
  - [instagram hashtag 搜尋頁](#instagram-hashtag-搜尋頁)
- [第四章 共載組件仔入篇](#第四章-共載組件仔入篇)
  - [非同步載入共用組件](#非同步載入共用組件)
  - [程式流程控管](#程式流程控管)
- [第五章 網址篇](#第五章-網址篇)
  - [網址轉址與重整](#網址轉址與重整)
  - [開心視窗](#開心視窗)
  - [網址 GET 參數](#網址GET參數)
  - [轉址帶參數注意事項](#轉址帶參數注意事項)
- [第六章 正規表達式入門](#第六章-正規表達式入門)
  - [while 迴圈](#while迴圈)
  - [for 迴圈](#for迴圈)
  - [全域變數與區域變數](#全域變數與區域變數)
- [第七章 ES6 升級指南](#第七章-ES6升級指南)
- [第八章 javaScript 模組化入門篇](#第八章-javaScript模組化入門篇)
- [第九章 javaScript 矯正姿勢篇](#第九章-javaScript矯正姿勢篇)

---

# 第一章 複雜資料進階篇

### 物件

> 網頁中任何元素都是物件，那怕是一個 p 標籤都是物件

```js
var obj1 = {};
console.log(obj1);
```

> object 寫法是大括弧

```js
var obj2 = {
  name: 'mike',
  age: 18,
  sex: 'male',
};

console.log(obj2);
console.log(obj2.name);
console.log(obj2.age);
console.log(obj2.sex);

obj.age = 12;
console.log(obj2.age);
```

> object 的讀取和存取可以直接賦值

```html
<body>
  <a id="alink" href="javascript:;" target="_blank">超連結標籤</a>
  <script>
    var dom = document.getElementById('alink');
    console.log(dom.id);
    console.log(dom.href);
    console.log(dom.target);
  </script>
</body>
```

**Object vs Array**

```js
var obj = {
  name: 'mike',
  age: 18,
  sex: 'male',
};
console.log(obj.name);

var arr = ['mike', 18, 'male'];
console.log(arr[0]);
```

## Object vs Array

| 項目             | Object                             | Array                         |
| ---------------- | ---------------------------------- | ----------------------------- |
| **資料結構**     | 無序的鍵值對 (Key-Value Pair)      | 有序的元素列表 (Indexed List) |
| **鍵的型別**     | 任意型別的鍵 (Key 必須是唯一的)    | 鍵為數字索引 (從 0 開始)      |
| **資料存取方式** | 使用鍵 (key) 存取資料              | 使用索引 (index) 存取資料     |
| **適用場景**     | 儲存結構化或命名資料，例如物件屬性 | 儲存列表、順序相關的資料      |
| **常見方法**     | `Object.keys()`、`Object.values()` | `push()`、`pop()`、`map()`    |
| **初始化語法**   | `{ key1: value1, key2: value2 }`   | `[value1, value2, value3]`    |
| **資料順序**     | 不保證順序                         | 保持插入順序                  |
| **操作效率**     | 適合需要快速查找鍵值的操作         | 適合需要依序操作資料的情境    |
| **重點特性**     | 各鍵對應的值可能是不同型別         | 所有元素通常屬於相同類型      |

**object 常用的使用方式**

```js
var obj = {
  name: 'mike',
};

console.log(obj.name);
console.log(obj['name']);

obj.sex = 'female';
console.log(obj.sex);
console.log(obj['sex']);
```

> 使用時機，需要動態的替換

```js
var obj = {
  name: 'mike',
  age: 18,
};

var text = 'name';
console.log(obj[text]);
```
