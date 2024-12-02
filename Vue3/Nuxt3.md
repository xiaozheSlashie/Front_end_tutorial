## 目錄

- [Nuxt3 安裝配置指南](#Nuxt3-安裝配置指南)

- [Nuxt3 專案功能擴充](#Nuxt3-專案功能擴充)

  - [pages](#pages)
  - [layouts](#layouts)
  - [public 與 assets](#public-與-assets)
  - [Components and Auto Imports](#Components-and-Auto-Imports)
  - [Setting Global](#Setting Global)

- [統整](#統整)

# Nuxt3 安裝配置指南

**安裝指令**

```shell
npx nuxi@latest init <project-name>
```

- Ok to proceed?(y): y
- Which package manager would you like to use?: npm
- Initialize git repository?: No

**確認專案是否安裝完成**

step1: 在 VSCode 開啟專案

step2: 在專案的終端機內輸入以下指令

```shell
npm run dev
```

# Nuxt3 專案功能擴充

**Nuxt3 是自助餐的形式，要什麼再擴充什麼**

[參考網址](https://nuxt.com/docs/api/commands/add)

## pages

```shell
npx nuxi add page page-name
```

### app.vue 改成

```vue
<template>
  <div>
    <NuxtPage />
  </div>
</template>
```

> 如果需要使用 pages/ 新增多個頁面，就需要使用 `<NuxtPage />` 當作網頁的進入點！

### 在 pages 的資料夾裡面，新增 index.vue

```vue
<script setup></script>
<template>
  <div>
    <h1>這是首頁</h1>
  </div>
</template>
<style scoped></style>
```

## layouts

```shell
npx nuxi add layout default
```

> 使用 layouts 就可以把 App.vue 給砍了，因為進入點就會從我們的 App.vue 變成 layouts/default.vue，更方便我們去客製化跟切換樣板。

## public-與-assets

> public => 靜態資源，不需要過編譯的檔案，例如 favicon.ico 或是一些 pdf 等等

> assets => 凡是需要透過編譯過的檔案都需要放在這邊，例如 css、image、svg 等等

**請自行新增一個 assets 的資料夾**

## Components and Auto Imports

```shell
npx nuxi add component Home
```

> 只要放在 components 資料夾底下就會自動引入，完全不需要 import

> 也可以使用資料夾的方式來新增 index.vue 的進入點，作為這個組件的使用

**Components 也可以像 pages 的用法一樣，在資料夾下面放 index.vue，這樣就在引入的時候就會優先找 index.vue 這檔案**

- 如果你的資料夾裡面有其他組件，要使用的話就要加上前面資料夾的名稱跟檔名

  - 例如:footer 資料夾裡的 Btn.vue
  - 引入:`<footerBtn/>`
