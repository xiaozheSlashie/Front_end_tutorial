## 目錄

- [定義資料](#定義資料)
  - [ref](#ref)
  - [reactive](#reactive)
  - [computed](#rcomputed)
- [物件處理](#物件處理)
  - [components](#components)
  - [props](#props)
  - [emit](#emit)

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

# 資料處理

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

---
