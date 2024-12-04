## 目錄

- [定義資料](#定義資料)
  - (ref)(#ref)
  - (reactive)(#reactive)
  - (computed)(#rcomputed)

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
