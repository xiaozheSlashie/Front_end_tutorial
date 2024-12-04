## 目錄

- [定義資料](#定義資料)
  - [ref](#ref)
  - [reactive](#reactive)
  - [computed](#rcomputed)
- [組件處理](#組件處理)
  - [components](#components)
  - [props](#props)
  - [emit](#emit)
  - [expose](#expose)
- [資料綁定](#資料綁定)
  - [v-model](#v-model)
    - [modifiers](#modifiers)
      - [.lazy](#lazy)
      - [.number](#number)
      - [.trim](#trim)
    - [可以綁定的元素](#可以綁定的元素)
  - [應用在組件互相傳遞資料](#應用在組件互相傳遞資料)
- [Composition API 和共用邏輯 Composables](#Composition-API-和共用邏輯-Composables)
- [Pinia](#Pinia)
  - [基礎用法](#基礎用法)
    - [未解構的方式](#未解構的方式)
    - [解構的方式](#解構的方式)
  - [監控用法](#監控用法)
    - [watch](#watch)
    - [subscribe](#subscribe)
- [Composables vs Pinia](#Composables-vs-Pinia)
- [Vue-Router](#Vue-Router)
  - [Vite](#Vite)
  - [Nuxt3](#Nuxt3)

# 定義資料

## ref

```vue
<script setup>
const name = 'Mike';
</script>
<template>
  {{ name }}
</template>
```

=> 無法更改 Name 的值

```vue
<script setup>
const name = ref('Mike');

settimeout(() => {
  name.value = 'John';
}, 1000);
</script>
<template>
  {{ name }}
</template>
```

=> 用 ref 這個涵式會回傳一個 promise 的物件，可以用.value 更改值

## reactive

```vue
<script setup>
const data = reactive([
  name: 'Mike',
  age: 12,
  email: 'A.A@gmail.com'
  ]); //只能放object和Array

  setTimeout(() => {
  data.name = 'John';
  data.age  = 20;
  data.emil = 'B.B@gmail.com';
}, 1000);
</script>
<template>
  {{ data.name }}
  {{ data.age }}
  {{ data.emil }}
</template>
```

=> 用 reactive 更改值不需透過.value

** ref v.s. reactive**

```vue
<script setup>
const num =ref(0)
const data3 = reactive([
  num:num;
])
console.log(data3.num); //會自動解包不需要用.value，recative每一個屬性等於ref的部分


const data1 = ref([
  name: 'John',
  age: 20,
  email: 'B.B@gmail.com'
  ]); //只能放object和Array
const data2 = reactive([
  name: 'Mike',
  age: 12,
  email: 'A.A@gmail.com'
  ]); //只能放object和Array

  setTimeout(() => {

  data1.value.name = 'John';
  data1.value.age  = 20;
  data1.value.emil = 'B.B@gmail.com';

  data2.name = 'John';
  data2.age  = 20;
  data2.emil = 'B.B@gmail.com';
}, 1000);


//第一個值是target，第二個值是callBack
watch(data1,()=>{
  console.log('ref物件被改變')
}); //不會console.log

//深度監聽
watch(data1,()=>{
  console.log('ref物件被改變')
},{deep: true}); //會console.log

watch(data2,()=>{
  console.log('reactive物件被改變')
});  //會console.log
</script>
<template>
  {{ data1.name }}
  {{ data1.age }}
  {{ data1.emil }}

  {{ data2.name }}
  {{ data2.age }}
  {{ data2.emil }}
</template>
```

|                     | **ref**                           | **recative**           |
| ------------------- | --------------------------------- | ---------------------- |
| 響應式功能          | 有                                | 有                     |
| 存放資料類型        | 任何型別資料                      | 只能放 object 和 array |
| 更改資料方式        | 透過.value                        | 不需要透過.value       |
| 是否能被 watch 監控 | 若是用 object 和 array 則無法監控 | 可以監控               |

## computed

```vue
<script setup>
const data = computed(() => {
  return {
    name = 'Mike',
    age = 12
  };
});
</script>
<template>
  {{ data.name }}
  {{ data.age }}
</template>
```

=> 拿來重組跟計算資料

```vue
<script setup>
const name = ref('Mike');
const data = computed(() => {
  return `我的名字叫{name.value}!`;
});
setTimeout(() => {
  name.value = 'John';
}, 2000);
</script>
<template>
  {{ data }}
</template>
```

**特性:只要裡面放有響應式的資料，若資料改變會自動偵測去改變**

```vue
<script setup>
const index = ref(0);
const data = computed(() => {
  if (index.value > 3) {
    return '這個大於3';
  } else {
    return '這個小於3';
  }
});
setTimeout(() => {
  index.value++;
}, 2000);
</script>
<template>
  {{ data }}
</template>
```

**注意**

1. computed 不能傳參數，要傳參數請用 function
2. computed 有 get 和 set 的功能

```vue
<script setup>
const count = ref(0);
const data = computed(() => {
  get: () => {
    return count.value;
  };
  set: (val) => {
    count.value = val;
  };
});
console.log('1=>', data.value);
data.value = 5;
console.log('2=>', data.value);
</script>
<template>
  {{ data }}
</template>
```

---

# 組件處理

## components

**1. 拆分每隔組件，避免把所有東西都塞在同一個.vue 檔案裡面，可以幫助專案做更好的管理**<br>

**2. 我們在[安裝 Vite](./Vite.md#components-組件自動引入)和[安裝 Nuxt3](./Nuxt3.md#Components-and-Auto-Imports)都有教安裝自動載入 components 的套件和使用，這裡就不再贅述**

### components 的載入

```vue
<script setup>
//用components :is的用法需使用import把component載入進來
import HelloWorld from '@/components/HelloWorld.vue';
import HelloMike from '@/components/HelloMike.vue';
import HelloJohn from '@/components/HelloJohn.vue';
import HelloJane from '@/components/HelloJane.vue';

const currentComponents = ref('HelloWorld');
const clickToChangeComponents = (component) => {
  currentComponents.value = component;
};

//記得不能return字串!!
const changeComponents = computed(() => {
  switch(currentComponents.value){
    case 'HelloWorld':
       return HelloWorld
    case 'HelloMike':
      return HelloMike
    case 'HelloJohn':
      return HelloJohn
    case 'HelloJane':
      return HelloJane
    default:
      return HelloWorld
  }

});
</script>
<template>
  <component :is="changeComponents" />
  <button @click="clickToChangeComponents('HelloWorld')">click to change
  HelloWorld component<button />
  <button @click="clickToChangeComponents('HelloMike')">click to change
   HelloMike component<button />
  <button @click="clickToChangeComponents('HelloJohn')">click to change
  HelloJohn component<button />
  <button @click="clickToChangeComponents('HelloJane')">click to change
  HelloJane component<button />
</template>
```

## props

**由父組件傳遞到子組件**

**父組件的.vue**

```vue
<script setup>
import Logo from './assets/img/logo.svg';
</script>
<template>
  <VImg alt="Vue Logo" :src="Logo" className="logo" width:"125" height:"125" />
</template>
```

**子組件的.vue (VImg.vue)**

```vue
<script setup>
// array
const propsArr = defineProps(['alt', 'className', 'src', 'width', 'height']);

// object
const propsObj = defineProps({
  alt: {
    type: String,
    default: 'IMG ALT',
  },
  className: {
    type: String,
    default: 'error',
  },
  src: {
    type: String,
    default: '',
  },
  width: {
    type: String,
    default: '125',
  },
  height: {
    type: String,
    default: '125',
  },
});
</script>
<template>
  <img
    :src="propArr.src"
    :alt="propsArr.alt"
    :class="propsArr.className"
    :width="propsArr.width"
    :height="propsArr.height"
  />

  <img
    :src="propObj.src"
    :alt="propsObj.alt"
    :class="propsObj.className"
    :width="propsObj.width"
    :height="propsObj.height"
  />
</template>
```

=> props 傳入 boolean

```js
const props = defineProps({
  bool: {
    type: Boolean,
    default: true,
  },
});
```

=> props 也可以傳入 object 和 array

```js
const props = defineProps({
  obj: {
    type: Object,
    default: () => ({}), //一行的形式
    default: () => {
      return;
    }, //很多行的形式
  },
  arr: {
    type: Array,
    deafult: () => [],
  },
});
```

=> props 也可以傳入 function

```js
const props = defineProps({
  fun: {
    type: function,
    default:()=>{}
  },
});
```

## emit

**由子組件傳遞到父組件**

**子組件的.vue(add.vue)**

```vue
<script setup>
const emit = defineEmits(['AddInt']);

const handleAddClick = (event, a = 1, b = 2) => {
  const result = a + b;
  emit('AddInt', result);
};
</script>
<template>
  <div>
    <button @click="andleAddClick(3, 5)">click</button>
  </div>
</template>
```

**父組件的.vue**

```vue
<script setup>
const resInt = ref(0);
const handleAddClick = (res) => {
  resInt.value = res;
};
</script>
<template>
  <Add @AddInt="handleAddClick" />
  {{ resInt }}
</template>
```

=> emit 可以定義涵式驗證傳上去的東西是否正確

** 子組件.vue**

```vue
<script setup>
const emit = defineEmits({
  AddInt: (res) => {
    if (res === 3) {
      return true; //要回傳
    } else {
      alert('傳遞參數錯誤!!');
      return false; //不要回傳
    }
  },
});

const handleAddClick = (event, a = 1, b = 2) => {
  const result = a + b;
  emit('AddInt', result);
};
</script>
<template>
  <div>
    <button @click="andleAddClick(3, 5)">click</button>
  </div>
</template>
```

## expose

**子組件向父組件暴露屬性和涵式，讓父組件可以直接使用子組件的屬性和涵式**

**defineExpose 沒有返回值**

**子組件的.vue**

```vue
<script setup>
const name = ref('John');
const sayName = () => {
  console.log('我的名字是 ' + name.value);
};
defineExpose({
  name,
  sayName,
});
</script>

<template>
  {{ name }}
</template>
```

**父組件的.vue**

```vue
<script setup>
const child = ref(null);
onMounted(() => {
  console.log(child.value.name); //John
  child.value.sayName(); //我的名字是 John
});
</script>

<template>
  <Child ref="child"></Child>
</template>
```

**注意:**

1. 當父組件直接使用子組件的屬性時會自動解包，所以不用在加.value

2. defineExpose 一定要在變數和涵式定義之後再使用(最好放在最下面)，不然會出錯!!

---

# 資料綁定

## v-model

**用來做資料的雙向綁定**

```vue
<script setup>
const userName = ref('Mike');
const clickSetName = (str) => {
  userName.value = str;
};
</script>
<template>
  <label for="userName">用戶名稱：</label>
  <input type="text" id="userName" v-model="userName" />
  {{ userName }}

  <button @click="clikcSetName('John')">click to change name</button>
</template>
```

### modifiers

#### lazy

**從 input 事件改成 change 事件**

> 裡面的東西確定被改動後才會被觸發

```vue
<script setup>
const userName = ref('Mike');
const clickSetName = (str) => {
  userName.value = str;
};
</script>
<template>
  <label for="userName">用戶名稱：</label>
  <input type="text" id="userName" v-model.lazy="userName" />
  <!--foucus在上面時並不會被更新-->
  {{ userName }}

  <button @click="clikcSetName('John')">click to change name</button>
</template>
```

#### number

**1. 字串強制轉型為數字**
**2. 若是輸入有含文字則不會轉型，只有輸入數字會(不能混合文字輸入)**
**3. 若開頭是 0 會把 0 給去掉(可用在電話號碼上面)**

```vue
<script setup>
const userAge = ref('12');
const clickSetAge = (num) => {
  userAge.value = num;
};
watch(userAge, (newValue) => {
  console.log(newValue);
});
</script>
<template>
  <label for="userAge">用戶年齡：</label>
  <input type="text" id="userAge" v-model.number="userAge" />
  {{ userAge }}

  <button @click="clikcSetName('20')">click to change Age</button>
</template>
```

#### trim

**避免使用者打出空格**

```vue
<script setup>
const userName = ref('Mike');
const clickSetName = (str) => {
  userName.value = str;
};
</script>
<template>
  <label for="userName">用戶名稱：</label>
  <input type="text" id="userName" v-model.trim="userName" />
  {{ userName }}

  <button @click="clikcSetName('John')">click to change name</button>
</template>
```

### 可以綁定的元素

> 能綁定就不要用{{value}}去寫，應該改成 v-model="value"

1. input
2. textarea
3. checkbox
4. radio
5. select

**checkbox 使用可以搭配 array**

```vue
<script>
const checkboxValue = ref([]);
</script>
<template>
  <p>checks name: {{ checkboxValue }}</p>

  <input type="checkbox" id="john" value="john" v-model="checkboxValue" />
  <label for="john">john</label>

  <input type="checkbox" id="jack" value="jack" v-model="checkboxValue" />
  <label for="jack">jack</label>

  <input type="checkbox" id="amy" value="amy" v-model="checkboxValue" />
  <label for="amy">amy</label>
</template>
```

## 應用在組件互相傳遞資料

**範例 1:**

**父組件**

```vue
<script setup>
const userName = ref('Mike');
</script>
<template>
  <Child v-model="userName"></Child>
  {{ userName }}
</template>
```

**子組件**

```vue
<script setup>
const props = defineProps(['modelValue']);
const emit = defineEmits(['Update:modelValue']);
</script>
<template>
  <label for="userName">用戶名稱：</label>
  <input
    type="text"
    id="userName"
    :value="props.modelValue"
    @input="$emit('Update:modelValue', $event.target.value)"
  />
</template>
```

> props 下來的值不可以在子組件修改，修改要去父組件修改

**範例 2:**

**父組件**

```vue
<script setup>
const userName = ref('Mike');
const handleChangeName = (res) => {
  userName.value = res;
};
</script>
<template>
  <Child :name="userNmae" @changeName="handleChangeName"></Child>
</template>
```

**子組件**

```vue
<script setup>
const props = defineProps({
  name: {
    type: String,
    default: '',
  },
});

const emit = defineEmits(['changeName']);
const clickToChangeName = () => {
  emit('changeName', 'Jack');
};
</script>
<template>
  <h1>props.name</h1>
  <button @click="clickToChangeName">click to change name</button>
</template>
```

**範例 3:**

> One-Way Data Flow 單向資料流

**父組件**

```vue
<script setup>
const name = ref('Mike');
const changeName = (res) => {
  name.value = res;
};
</script>
<template>
  <child :name="name" :changeName="changeName"> </child>
</template>
```

**子組件**

```vue
<script setup>
const props = defineProps({
  name: {
    type: String,
    default: '',
  },
  changeName: {
    type: Function,
    default: () => {},
  },
});
</script>
<template>
  <h1>{{ props.name }}</h1>
  <button @click="props.changeName('Jacky')">click to change name</button>
</template>
```

---

# Composition API 和共用邏輯 Composables

> 新建一個 Composables 的資料夾，在裡面放入.js 檔案(共用邏輯檔案)

**範例:想要抓取滑鼠座標**

**在 useCatchMousePosition.js**

```js
import { onMounted, onUnmounted, ref, readonly } from 'vue';
export const useCatchMousePosition = () => {
  const pageX = ref(0);
  const pageY = ref(0);

  const movePosition = (e) => {
    pageX.value = e.pageX;
    pageY.value = e.pageY;
  };
  onMounted(() => {
    window.addEventListener('mouseover', movePosition);
  });

  onUnmounted(() => {
    window.removeEventListener('mouseover', movePosition);
  });

  return {
    pageX: readonly(pageX), //只可讀取
    pageY: readonly(pageY),
  };
};
```

> 要設定要再建立一個 setPageX 的涵式去做設定，不要直接改值

**在要使用的.vue**

```vue
<script setup>
import { useCatchMousePosition } from '@/composables/useCatchMousePosition.js';
const { pageX, pageY } = useCatchMousePosition();
</script>
<template>
  {{ pageX }}
  {{ pageY }}
</template>
```

**API 處理**

**下載 axios**

```shell
npm install axios
```

**.js 檔案**

```js
import axios from 'axios';
export const useFetchCard = () => {
  const errorMsg = ref(null);
  const data = ref([]);
  const fetchInit = async () => {
    try {
      const res = await axios.get(
        'https://vue-lessons-api.vercel.app/courses/list'
      );
      data.value = res.data;
    } catch (error) {
      errorMsg.value = 'API 發生錯誤!!';
    }
  };
  return {
    data: readonly(data),
    errorMsg: readonly(errorMsg),
    fetchInit,
  };
};
```

**.vue 檔案**

```vue
<script setup>
import { useFetchCard } from '@/composables/useFetchCard.js';
const { data, errorMsg, fetchInit } = useFetchCard();
onMounted(() => {
  fetchInit();
});
</script>
<template>
  <h1 v-if="data.length === 0">loading...</h1>
  <div v-else>
    {{ data }}
  </div>
  <h1 v-if="errorMsg !== null">{{ errorMsg }}</h1>
</template>
```

---

# Pinia

**安裝指令可以去[安裝 Vite](./Vite#全域資料管理-pinia-已安裝可跳過.md)和[安裝 Nuxt3](./Nuxt3#pinia-安裝.md)查看**

**stores/count.js**

```js
import { defineStore } from 'pinia'; //Nuxts3不用加這行

export const useCountStore = defineStore('Count', () => {
  const count = ref(0);
  const double = computed(() => count.value * 2);
  const addCount = () => {
    count.value++;
  };
  return {
    count,
    double,
    addCount,
  };
});
```

**在使用的.vue**

## 未解構的方式

```vue
<script setup>
import useCountStore from '@/store/count.js'; //Nuxts3不用加這行
const countStore = useCountStore();
</script>
<template>
  {{ countStore.count }}
  {{ countStore.double }}
  <button @click="newsStore.addCount()">click to add</button>
</template>
```

## 解構的方式

```vue
<script setup>
import { storeToRefs } from 'pinia'; //Nuxts3不用加這行
import useCountStore from '@/store/count.js'; //Nuxts3不用加這行
const countStore = useCountStore();
const { addCount } = useCountStore(); // function可以直接解構
const { count, double } = storeToRefs(countStore);
</script>
<template>
  {{ count }}
  {{ double }}
  <button @click="addCount()">click to add</button>
</template>
```

1. 若是值要先 `import { storeToRefs } from 'pinia';`再用 storeToRefs 解構

```js
import { storeToRefs } from 'pinia'; //Nuxts3不用加這行
import useCountStore from '@/store/count.js'; //Nuxts3不用加這行
const countStore = useCountStore();
const { count, double } = storeToRefs(countStore);
```

2. 若是 function 可以直接解構

```js
import useCountStore from '@/store/count.js'; //Nuxts3不用加這行
const { addCount } = useCountStore(); // function可以直接解構
```

**Pinia 的.js 互相使用**

**stores/about.js**

```js
import { defineStore } from 'pinia'; //Nuxts3不用加這行

export const useAboutStore = defineStore('About', () => {
  const name = ref('Mike'); // 內部可修改的變量
  const setName = (str) => {
    name.value = str;
  };
  return {
    name: readonly(name),
    setName,
  };
});
```

**stores/count.js**

```js
import { defineStore } from 'pinia'; //Nuxts3不用加這行
import { storeToRefs } from 'pinia'; //Nuxts3不用加這行
import { useAboutStore } from '@/store/useAboutStore.js'; //Nuxts3不用加這行
export const useCountStore = defineStore('Count', () => {
  const count = ref(0);
  const aboutStore = useAboutStore();
  const { setName } = useAboutStore();
  const { name } = storeToRefs(aboutStore);
  const double = computed(() => count.value * 2 + name.value);
  const addCount = () => {
    count.value++;
    setName('Jack');
  };
  return {
    count,
    double,
    addCount,
  };
});
```

## Nuxt3 都不需要 import，import 會出錯!!

## 監控功能

### watch

```js
watch(
  double,
  (newValue, oldValue) => {
    console.log('newValue=>', newValue, 'OldValue=>', oldValue);
  },
  { deep: true }
);
```

### subscribe

```js
import { storeToRefs } from 'pinia'; //Nuxts3不用加這行
import useCountStore from '@/store/count.js'; //Nuxts3不用加這行
const countStore = useCountStore();
const { addCount } = useCountStore(); // function可以直接解構
const { count, double } = storeToRefs(countStore);
countStore.$subscribe((mutation, state) => {
  console.log(mutation);
});
```

---

# Composables vs Pinia

| **特性**       | **Composables**                                                 | **Pinia**                                                          |
| -------------- | --------------------------------------------------------------- | ------------------------------------------------------------------ |
| **用途**       | 用於封裝和處理共用邏輯，無狀態管理特化功能。                    | 用於集中化的全域資料管理（Vue 3 的官方推薦解決方案）。             |
| **依賴性**     | 標準 Vue 3 Composition API，可直接使用。                        | 必須安裝 `@pinia/nuxt` 或 `pinia` 模組。                           |
| **適用範圍**   | 在單一或多個元件間共享邏輯和功能（例如 API 請求、計算屬性等）。 | 適用於應用程式全域的狀態共享和數據管理。                           |
| **數據持久化** | 不提供數據持久化，需要手動實現（例如本地存儲）。                | 支援第三方插件進行數據持久化（如 `pinia-plugin-persistedstate`）。 |
| **調試工具**   | 無專用調試工具，需使用 Vue DevTools 查看相關數據。              | 與 Vue DevTools 無縫集成，可查看和修改狀態。                       |
| **結構化**     | 自由度高，可根據需求任意結構化組織。                            | 具有嚴格的 Store 結構，包含 `state`、`getters` 和 `actions`。      |
| **類型支援**   | 手動實現類型定義（如使用 TypeScript）。                         | 原生支援 TypeScript，提供自動補全和類型檢查。                      |
| **性能**       | 適用於小型邏輯模組，共享範圍小且輕量。                          | 適用於大型應用的狀態管理，可支援多個模組且性能優化良好。           |
| **學習曲線**   | 基於 Composition API，學習門檻低，靈活性高。                    | 需要學習 Pinia 特有 API，如 `defineStore`。                        |
| **更新觸發**   | 通常依賴 `ref` 或 `reactive`，手動控制響應式數據更新。          | 自動跟蹤狀態更改，且支援批量更新，避免多餘的重渲染。               |
| **使用範例**   | 用於封裝 API 請求邏輯、計算屬性或工具函數。                     | 用於存儲用戶狀態、全局設置或多組件共享的大量數據。                 |
| **額外依賴**   | 不需要額外的依賴，僅需 Vue 3 原生支持。                         | 需要引入 Pinia 模組，並在應用中註冊。                              |

---

# Vue-Router

## Vite

```vue
<template>
  <RouterLink to="/"> Home <RouterLink/>
  <RouterLink to="/about"> About <RouterLink/>
</template>
```

**router/index.js**

```js
import { createMemoryHistory, createRouter } from 'vue-router';

import HomeView from './HomeView.vue';
import AboutView from './AboutView.vue';

const routes = [
  { path: '/', component: HomeView },
  { path: '/about', component: AboutView },
];

const router = createRouter({
  history: createMemoryHistory(),
  routes,
});
```

[vue router 官方網站](https://router.vuejs.org/zh/)

## Nuxt3

```vue
<template>
  <NuxtLink to="/"> Home page </NuxtLink>
  <NuxtLink to="/about"> About page </NuxtLink>
</template>
```

### 帶參數

```vue
<template>
  <NuxtLink :to="{ name: 'posts-id', params: { id: 123 } }">
    Post 123
  </NuxtLink>
</template>
```

### download pdf

```vue
<template>
  <NuxtLink to="/the-important-report.pdf" external> Download Report </NuxtLink>
</template>
```

### 外部網址

```vue
<template>
  <NuxtLink to="https://nuxtjs.org"> Nuxt website </NuxtLink>
  <!-- <a href="https://nuxtjs.org" rel="noopener noreferrer">...</a> -->
</template>
```

### 也可加 a 的額外參數

```vue
<template>
  <NuxtLink to="https://twitter.com/nuxt_js" target="_blank">
    Nuxt Twitter
  </NuxtLink>
  <!-- <a href="https://twitter.com/nuxt_js" target="_blank" rel="noopener noreferrer">...</a> -->

  <NuxtLink to="https://discord.nuxtjs.org" target="_blank" rel="noopener">
    Nuxt Discord
  </NuxtLink>
  <!-- <a href="https://discord.nuxtjs.org" target="_blank" rel="noopener">...</a> -->

  <NuxtLink to="https://github.com/nuxt" no-rel> Nuxt GitHub </NuxtLink>
  <!-- <a href="https://github.com/nuxt">...</a> -->

  <NuxtLink to="/contact" target="_blank">
    Contact page opens in another tab
  </NuxtLink>
  <!-- <a href="/contact" target="_blank" rel="noopener noreferrer">...</a> -->
</template>
```
