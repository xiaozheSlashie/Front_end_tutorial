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

  settimeout(() => {
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

  settimeout(() => {

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
