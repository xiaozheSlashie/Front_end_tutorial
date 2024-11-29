# 目錄

- [第一章 複雜資料進階篇](#第一章-複雜資料進階篇)
  - [物件](#物件)
  - [陣列](#陣列)
  - [資料格式的基本觀念](#資料格式的基本觀念)
  - [function 回傳](#function-回傳)
  - [操作資料](#操作資料)
  - [陣列進階篇](#陣列進階篇)
    - [forEach](#forEach)
    - [filter](#filter)
    - [every](#every)
    - [map](#map)
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

### Object vs Array

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

### 陣列包物件

```js
[
  {
    // ....
  },
  {
    // ....
  },
  {
    // ....
  },
];
```

> 多筆資料裡面又有屬性的時候就會使用陣列包物件

**範例:**

```js
var Staff = [
  {
    name: 'mike',
    sex: 'male',
    age: '18',
    salary: '22000',
  },
  {
    name: 'jacky',
    sex: 'male',
    age: '18',
    salary: '76000',
  },
  {
    name: 'andy',
    sex: 'male',
    age: '40',
    salary: '80000',
  },
  {
    name: 'scars',
    sex: 'male',
    age: '30',
    salary: '85000',
  },
  {
    name: 'ash',
    sex: 'female',
    age: '25',
    salary: '78000',
  },
];

console.log(Staff);
```

### 陣列迴圈應用

1. 範例一

**原本陣列寫法**

```js
var html = '';
var title = [
  '會計年度最後一個月 美國防部狂花1.4億買蝦蟹...',
  '大逆轉！情侶照1.5萬變16萬 當事人道歉了...',
  '台南是民主聖地？謝龍介：變民主很火大...',
  '槓王選立委！韓笑回他長大了 潘恆旭尷尬回應...',
  '捷運西門站藥妝搶駐！日藥妝210萬標下6店面...',
  '她全身麻醉動刀 苦求男友陪！遭嗆：我要上班...',
  '自閉症青年音樂家亞洲巡迴 精彩演出獲好評...',
  '小黃司機遭3人挾持30公里！傳LINE求救逃脫...',
];
var url = [
  'https://news.ebc.net.tw/News/Article/155505',
  'https://news.ebc.net.tw/News/Article/155504',
  'https://news.ebc.net.tw/News/Article/155503',
  'https://news.ebc.net.tw/News/Article/155502',
  'https://news.ebc.net.tw/News/Article/155501',
  'https://news.ebc.net.tw/News/Article/155500',
  'https://news.ebc.net.tw/News/Article/155499',
  'https://news.ebc.net.tw/News/Article/155497',
];

for (var i = 0; i < title.length; i++) {
  html +=
    '<li><a href="' + url[i] + '" target="_blank">' + title[i] + '</a></li>';
}
document.getElementById('list').innerHTML = html;
```

** 轉換成 object 寫法**

```js
var html = '';
var news = [
  {
    title: '會計年度最後一個月 美國防部狂花1.4億買蝦蟹...',
    url: 'https://news.ebc.net.tw/News/Article/155505',
  },
  {
    title: '大逆轉！情侶照1.5萬變16萬 當事人道歉了...',
    url: 'https://news.ebc.net.tw/News/Article/155504',
  },
  {
    title: '台南是民主聖地？謝龍介：變民主很火大...',
    url: 'https://news.ebc.net.tw/News/Article/155503',
  },
  {
    title: '槓王選立委！韓笑回他長大了 潘恆旭尷尬回應...',
    url: 'https://news.ebc.net.tw/News/Article/155502',
  },
  {
    title: '捷運西門站藥妝搶駐！日藥妝210萬標下6店面...',
    url: 'https://news.ebc.net.tw/News/Article/155501',
  },
  {
    title: '她全身麻醉動刀 苦求男友陪！遭嗆：我要上班...',
    url: 'https://news.ebc.net.tw/News/Article/155499',
  },
  {
    title: '小黃司機遭3人挾持30公里！傳LINE求救逃脫...',
    url: 'https://news.ebc.net.tw/News/Article/155497',
  },
];

for (var i = 0; i < news.length; i++) {
  html +=
    '<li><a href="' +
    news[i].url +
    '" target="_blank">' +
    news[i].title +
    '</a></li>';
}

document.getElementById('list').innerHTML = html;
```

### 物件包陣列

**範例:做一個搜尋引擎出來**

> 步驟 1: 先做完點擊 Trending searches:時可以跑出相關列表

> 步驟 2: 再做打 input 後可以拿到搜尋列表

**因應你的需求也會有這種物件裡面是陣列的複合式使用方式，key 支援中文，不過要用 "" 給包起來，這種做法也是很常見!**

```html
<body>
  <div class="mid">
    <input class="search" type="text" value="Online Courses" />
    <p>
      Trending searches: <a class="tag" href="javascript:;">Matsu</a>、
      <a class="tag" href="javascript:;">Earthquake</a>、
      <a class="tag" href="javascript:;">Curator</a>、
      <a class="tag" href="javascript:;">Online Courses</a>
    </p>
    <div class="search_list">
      <!-- <div class="item">
                  <a href="javascript:;" target="_blank" class="title">Matsu Belief in Taiwan - Wikipedia, the Free Encyclopedia</a>
                  <a href="javascript:;" target="_blank" class="link">https://zh.wikipedia.org/zh-tw/台灣媽祖信仰</a>
                  <p>媽The belief in Matsu is a common folk belief in Taiwan. Early immigrants came from South China, crossing the seas with trepidation. Matsu, the sea goddess, became an important spiritual symbol for the Taiwanese people...</p>
              </div> -->
    </div>
  </div>
  <script>
    var searchData = {
      Matsu: [
        {
          title: 'Matsu Belief in Taiwan - Wikipedia, the Free Encyclopedia',
          link: 'https://zh.wikipedia.org/zh-tw/台灣媽祖信仰',
          text: 'The belief in Matsu is a common folk belief in Taiwan. Early immigrants came from South China, crossing the seas with trepidation. Matsu, the sea goddess, became an important spiritual symbol for the Taiwanese people...',
        },
        {
          title: 'Introduction to Matsu',
          link: 'www.ntcu.edu.tw/edison/otm/__1.html',
          text: "Matsu, also known as the 'Heavenly Mother,' is a widely respected deity in Taiwan. Her real name was Lin Mo-Niang, known for her intelligence and strong memory of scriptures...",
        },
      ],
      Earthquake: [
        {
          title: 'Earthquake - Central Weather Bureau',
          link: 'https://www.cwb.gov.tw/V7/earthquake/',
          text: 'Data Source: Earthquake auto-location information released by the US Geological Survey. This webpage only shows global earthquakes with a magnitude of 6 or above...',
        },
      ],
      Curator: [
        {
          title: 'Curator Genghis Khan - YouTube',
          link: 'https://www.youtube.com/channel/UCnnp2fWa77PP2h08T7WAzzw',
          text: 'The paradise of fitness and martial arts enthusiasts. Genghis Khan Fitness Club and Evolution MMA Center...',
        },
        {
          title: 'Chen Zhihan - Wikipedia, the Free Encyclopedia',
          link: 'https://zh.wikipedia.org/zh-tw/陳之漢',
          text: "Chen Zhihan (born March 12, 1979), also known as 'Curator,' is a Taiwanese internet celebrity, entrepreneur, fitness coach, and founder of a fitness chain...",
        },
      ],
      'Online Courses': [
        {
          title: 'Modern JavaScript Path｜Beginner Edition',
          link: 'https://hiskio.com/courses/244',
          text: 'Learn the fundamentals of JavaScript with hands-on practice and practical examples. This course is designed to help you become a professional front-end developer!',
        },
        {
          title: 'Modern JavaScript Path｜Intermediate Edition',
          link: 'https://hiskio.com/courses/245',
          text: 'Advance your skills with expert mentorship and real-world projects to become a proficient front-end engineer.',
        },
        {
          title: 'Responsive Web Design Essentials',
          link: 'https://hahow.in/cr/responsive-design',
          text: 'Master the principles of responsive design with practical examples, addressing common challenges like layout issues on mobile devices...',
        },
      ],
    };

    var searchObj = searchData['Online Courses'];
    var search = document.getElementsByClassName('search')[0];
    var searchList = document.getElementsByClassName('search_list')[0];
    var tags = document.getElementsByClassName('tag');
    var html = '';

    // DOM rendering
    function DOMrender() {
      html = '';
      searchList.innerHTML = '';
      for (var i = 0; i < searchObj.length; i++) {
        html += '<div class="item">';
        html +=
          '<a href="javascript:;" target="_blank" class="title">' +
          searchObj[i].title +
          '</a>';
        html +=
          '<a href="' +
          searchObj[i].link +
          '" target="_blank" class="link">' +
          searchObj[i].link +
          '</a>';
        html += '<p>' + searchObj[i].text + '</p>';
        html += '</div>';
      }
      searchList.innerHTML = html;
    }

    // Click event for tags
    for (var s = 0; s < tags.length; s++) {
      tags[s].addEventListener('click', searchTagChange);
    }

    // Update data when a tag is clicked
    function searchTagChange() {
      searchObj = searchData[this.innerText];
      search.value = this.innerText;
      DOMrender();
    }

    // Keyup event for the input field
    console.log(search);
    search.addEventListener('keyup', function () {
      searchObj = searchData[this.value];
      if (searchObj === undefined) {
        searchObj = [];
      }
      DOMrender();
    });

    // Initialize
    DOMrender();
  </script>
</body>
```

### 資料格式的基本觀念

```js
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

var obj = {
  name: 'mike',
};

var arrArr = [
  ['2019/01/01', 'https://www.cwb.gov.tw/V7/earthquake/'],
  ['2019/01/02', 'https://www.cwb.gov.tw/V7/earthquake/'],
];

var arrObj = [
  {
    date: '2019/01/01',
    url: 'https://www.cwb.gov.tw/V7/earthquake/',
  },
  {
    date: '2019/01/02',
    url: 'https://www.cwb.gov.tw/V7/earthquake/',
  },
  {
    date: '2019/01/03',
    url: 'https://www.cwb.gov.tw/V7/earthquake/',
  },
];

var nested = {
  idx: '1',
  date: '2019/01/01',
  userList: [
    { name: 'mike' },
    { name: 'jacky' },
    { name: 'andy' },
    { name: 'scars' },
    { name: 'ash' },
  ],
};
```

### function 回傳

**點擊 btn 拿資料**

> e 為滑鼠事件的東西，可以操作點擊後的所有東西

> this == e.Target

```js
document.getElementById('btn').addEventListener('click', function (e) {
  console.log(e);
});

function Test() {
  return 1 + 1;
}
console.log(Test());

function propsAdd(a, b) {
  return a + b;
}
console.log(propsAdd(1, 1));
```

> 當程式執行的時候就會返回停止，不會再繼續執行了

> return 要在涵式裡執行，不能在涵式外面執行

### 操作資料

#### 原本操作:既要操作資料又要操作頁面

#### 現在操作:只要操作資料，讓資料自己去操作頁面

> 讓資料和邏輯切開

**原先範例**

```js
var menu = document.getElementsByClassName('menu')[0];
var menuBtn = document.getElementsByClassName('menuBtn')[0];
var closeBtn = document.getElementsByClassName('closeBtn')[0];
menuBtn.addEventListener('click', function () {
  menu.classList.add('open');
});
closeBtn.addEventListener('click', function () {
  menu.classList.remove('open');
});
```

**用 Object​.define​Property()修改後的範例**

> Object​.define​Property() 可以監控 Object 的 api，Object 一有變化就會去執行

> Object​.define​Property(data,key,object)

**參數說明:**

1. data: 綁定的資料

2. key: 偵測哪個值變化時做改變

3. object: 物件設定(get[拿]，set[取])

   - 後面沒有賦予(assign)值就是 get
   - 賦予(assign)值就是 set

##### 範例一:

```js
var menu = document.getElementsByClassName('menu')[0];
var menuBtn = document.getElementsByClassName('menuBtn')[0];
var closeBtn = document.getElementsByClassName('closeBtn')[0];

var data = {
  isMenuOpen: false,
};

Object.defineProperty(data, 'isMenuOpen', {
  get: function () {
    return isMenuOpen;
  },
  set: function (value) {
    isMenuOpen = value;
    handClass();
  },
});

function handClass() {
  //後面沒有賦予(assign)值就是get
  if (data.isMenuOpen) {
    menu.classList.add('open');
  } else {
    menu.classList.remove('open');
  }
}

//====================================================================================

//賦予(assign)值就是set
menuBtn.addEventListener('click', function () {
  data.isMenuOpen = true;
});
closeBtn.addEventListener('click', function () {
  data.isMenuOpen = false;
});
```

##### 範例二:更改 05 的範例改為用 Object​.define​Property()

```html
<body>
  <div class="mid">
    <input class="search" type="text" value="Online Courses" />
    <p>
      Trending searches: <a class="tag" href="javascript:;">Matsu</a>、
      <a class="tag" href="javascript:;">Earthquake</a>、
      <a class="tag" href="javascript:;">Curator</a>、
      <a class="tag" href="javascript:;">Online Courses</a>
    </p>
    <div class="search_list">
      <!-- <div class="item">
                  <a href="javascript:;" target="_blank" class="title">Matsu Belief in Taiwan - Wikipedia, the Free Encyclopedia</a>
                  <a href="javascript:;" target="_blank" class="link">https://zh.wikipedia.org/zh-tw/台灣媽祖信仰</a>
                  <p>媽The belief in Matsu is a common folk belief in Taiwan. Early immigrants came from South China, crossing the seas with trepidation. Matsu, the sea goddess, became an important spiritual symbol for the Taiwanese people...</p>
              </div> -->
    </div>
  </div>
  <script>
    var searchData = {
      Matsu: [
        {
          title: 'Matsu Belief in Taiwan - Wikipedia, the Free Encyclopedia',
          link: 'https://zh.wikipedia.org/zh-tw/台灣媽祖信仰',
          text: 'The belief in Matsu is a common folk belief in Taiwan. Early immigrants came from South China, crossing the seas with trepidation. Matsu, the sea goddess, became an important spiritual symbol for the Taiwanese people...',
        },
        {
          title: 'Introduction to Matsu',
          link: 'www.ntcu.edu.tw/edison/otm/__1.html',
          text: "Matsu, also known as the 'Heavenly Mother,' is a widely respected deity in Taiwan. Her real name was Lin Mo-Niang, known for her intelligence and strong memory of scriptures...",
        },
      ],
      Earthquake: [
        {
          title: 'Earthquake - Central Weather Bureau',
          link: 'https://www.cwb.gov.tw/V7/earthquake/',
          text: 'Data Source: Earthquake auto-location information released by the US Geological Survey. This webpage only shows global earthquakes with a magnitude of 6 or above...',
        },
      ],
      Curator: [
        {
          title: 'Curator Genghis Khan - YouTube',
          link: 'https://www.youtube.com/channel/UCnnp2fWa77PP2h08T7WAzzw',
          text: 'The paradise of fitness and martial arts enthusiasts. Genghis Khan Fitness Club and Evolution MMA Center...',
        },
        {
          title: 'Chen Zhihan - Wikipedia, the Free Encyclopedia',
          link: 'https://zh.wikipedia.org/zh-tw/陳之漢',
          text: "Chen Zhihan (born March 12, 1979), also known as 'Curator,' is a Taiwanese internet celebrity, entrepreneur, fitness coach, and founder of a fitness chain...",
        },
      ],
      'Online Courses': [
        {
          title: 'Modern JavaScript Path｜Beginner Edition',
          link: 'https://hiskio.com/courses/244',
          text: 'Learn the fundamentals of JavaScript with hands-on practice and practical examples. This course is designed to help you become a professional front-end developer!',
        },
        {
          title: 'Modern JavaScript Path｜Intermediate Edition',
          link: 'https://hiskio.com/courses/245',
          text: 'Advance your skills with expert mentorship and real-world projects to become a proficient front-end engineer.',
        },
        {
          title: 'Responsive Web Design Essentials',
          link: 'https://hahow.in/cr/responsive-design',
          text: 'Master the principles of responsive design with practical examples, addressing common challenges like layout issues on mobile devices...',
        },
      ],
    };

    var search = document.getElementsByClassName('search')[0];
    var search_list = document.getElementsByClassName('search_list')[0];
    var tag = document.getElementsByClassName('tag');
    var html = '';

    var searchObj = {
      item: [],
    };

    Object.defineProperty(searchObj, 'item', {
      get: function () {
        return item;
      },
      set: function (value) {
        DOMrender(value);
      },
    });

    // DOM 元素的操作
    function DOMrender(value) {
      var step = value;
      if (value === undefined) {
        step = [];
      }
      html = '';
      search_list.innerHTML = '';
      for (var i = 0; i < step.length; i++) {
        html += '<div class="item">';
        html +=
          '<a href="javascript:;" target="_blank" class="title">' +
          step[i].title +
          '</a>';
        html +=
          '<a href="' +
          step[i].link +
          '" target="_blank" class="link">' +
          step[i].link +
          '</a>';
        html += '<p>' + step[i].text + '</p>';
        html += '</div>';
      }
      search_list.innerHTML = html;
    }

    //點擊標籤
    for (var s = 0; s < tag.length; s++) {
      tag[s].addEventListener('click', searchTagChange);
    }

    // 更改點到的資料
    function searchTagChange() {
      searchObj.item = searchData[this.innerText];
      search.value = this.innerText;
    }

    //鍵盤事件
    search.addEventListener('keyup', function () {
      searchObj.item = searchData[this.value];
    });

    // 初始化執行
    searchObj.item = searchData['Online Courses'];
  </script>
</body>
```

##### 範例三: 練習讓 Object​.define​Property()吃多個屬性

```html
<body>
  <button id="btn1">Change Title</button>
  <button id="btn2">Change Link</button>
  <button id="btn3">Cange Text</button>
  <button id="btn4">Cange Money</button>

  <script>
    var data = {
      title: 'Modern JavaScript Path｜Intermediate Edition',
      link: 'https://hiskio.com/courses/245',
      text: 'You need an instructor with years of industry experience and teaching expertise to guide you from basics to becoming a skilled front-end engineer.',
      money: '3800',
    };

    function BindValue(obj, key, callback) {
      var val = obj[key];
      Object.defineProperty(obj, key, {
        get: function () {
          return val;
        },
        set: function (value) {
          val = value;
          callback(val);
        },
      });
    }

    BindValue(data, 'title', handleTitle);
    BindValue(data, 'link', handleLink);
    BindValue(data, 'text', handleText);
    BindValue(data, 'money', handleMoney);

    function handleTitle(val) {
      console.log(data);
    }

    function handleLink(val) {
      console.log(data);
    }

    function handleText(val) {
      console.log(data);
    }

    function handleMoney(val) {
      console.log(data);
    }

    document.getElementById('btn1').addEventListener('click', function () {
      data.title = 'Responsive Web Design Essentials';
    });

    document.getElementById('btn2').addEventListener('click', function () {
      data.link = 'https://hahow.in/cr/responsive-design';
    });

    document.getElementById('btn3').addEventListener('click', function () {
      data.text =
        'Start from scratch and build a strong foundation with hands-on practice. Learn to solve common mobile layout issues and master responsive design techniques.';
    });

    document.getElementById('btn4').addEventListener('click', function () {
      data.money = '2000';
    });
  </script>
</body>
```

### 陣列進階篇

#### forEach

**forEach()  方法會將陣列內的每個元素，皆傳入並執行給定的函式一次。**

用法:

```js
array.forEach(function (currentValue, index, array) {
  // 你的邏輯
}, thisArg);
```

參數說明：

- currentValue: 正在處理的陣列元素。
- index（可選）: 正在處理的元素的索引。
- array（可選）: 正在操作的陣列本身。
- thisArg（可選）: 執行回呼函數時 this 的值。

**範例:**

> 引入資料

> 將 for 迴圈改寫成 forEach

> 試著用 forEach 寫出 filter 的效果

```html
<body>
  <div id="app">
    <ul class="tit_select">
      <li></li>
      <li>
        <select id="user_money">
          <option value="0">Select Money</option>
          <option value="300">Greater than 300</option>
          <option value="700">Greater than 700</option>
          <option value="10000">Greater than 10000</option>
        </select>
      </li>
      <li>
        <select id="user_age">
          <option value="0">Select Age</option>
          <option value="10">Older than 10</option>
          <option value="20">Older than 20</option>
          <option value="30">Older than 30</option>
        </select>
      </li>
      <li>
        <select id="user_sex">
          <option value="no">Select Gender</option>
          <option value="Male">Male</option>
          <option value="Female">Female</option>
        </select>
      </li>
    </ul>
    <ul class="tit">
      <li>Name</li>
      <li>Account Money</li>
      <li>Age</li>
      <li>Gender</li>
    </ul>
    <div class="itemList">
      <!-- <ul class="tit2">
                <li>John</li>
                <li>12000</li>
                <li>12</li>
                <li>Male</li>
            </ul> -->
    </div>
  </div>

  <script src="./js/data.js"></script>
  <script>
    var itemList = document.getElementsByClassName('itemList')[0];
    var user_money = document.getElementById('user_money');
    var user_age = document.getElementById('user_age');
    var user_sex = document.getElementById('user_sex');
    var html = '';
    var newData = data;

    // Render the UI
    function DOMReader(arr) {
      html = '';
      itemList.innerHTML = '';
      arr.forEach(function (obj) {
        html += '<ul class="tit2">';
        html += '<li>' + obj.name + '</li>';
        html += '<li>' + obj.money + '</li>';
        html += '<li>' + obj.age + '</li>';
        html += '<li>' + obj.sex + '</li>';
        html += '</ul>';
      });
      itemList.innerHTML = html;
    }

    // Handle money filter
    function userMoney(value) {
      var m = Number(value);
      var arr = [];
      newData.forEach(function (obj) {
        if (obj.money > m) {
          arr.push(obj);
        }
      });
      DOMReader(arr);
    }

    // Handle age filter
    function userAge(value) {
      var m = Number(value);
      var arr = [];
      newData.forEach(function (obj) {
        if (obj.age > m) {
          arr.push(obj);
        }
      });
      DOMReader(arr);
    }

    // Handle gender filter
    function userSex(value) {
      var arr = [];
      newData.forEach(function (obj) {
        if (obj.sex === value) {
          arr.push(obj);
        }
        if (value === 'no') {
          arr.push(obj);
        }
      });
      DOMReader(arr);
    }

    user_money.addEventListener('change', function () {
      userMoney(this.value);
    });
    user_age.addEventListener('change', function () {
      userAge(this.value);
    });
    user_sex.addEventListener('change', function () {
      userSex(this.value);
    });

    DOMReader(newData);
  </script>
</body>
```

**this 指向: 對於 function 被哪個監聽所調用去指向那個監聽，不能放在 forEach 裡面!!**

> 當 this 沒有指向被監聽的 function 時，會一律指向最外層的 window
