<script setup>
import { ref, computed, watch } from 'vue';
import "bootstrap/dist/css/bootstrap.css";
import '@fortawesome/fontawesome-free/css/all.css';
import Search from './components/search.vue';
import uTable from './components/uBikeTable.vue';
import Pagination from './components/pagination.vue';

// 修改這份 YouBike 即時資訊表：
// 1. 將搜尋的部分拆出來變成子元件 `uBikeTable/components/search.vue`
// 2. 將表格的部分拆出來變成子元件 `uBikeTable/components/uBikeTable.vue`
// 3. 將分頁的部分拆出來變成子元件 `uBikeTable/components/pagination.vue`
// 4. 再將它們組合起來


// 欄位說明:
// https://data.taipei/dataset/detail?id=c6bc8aed-557d-41d5-bfb1-8da24f78f2fb
// sno：站點代號、 sna：場站名稱(中文)、 tot：場站總停車格、
// sbi：場站目前車輛數量、 sarea：場站區域(中文)、 mday：資料更新時間、
// lat：緯度、 lng：經度、 ar：地(中文)、 sareaen：場站區域(英文)、
// snaen：場站名稱(英文)、 aren：地址(英文)、 bemp：空位數量、 act：全站禁用狀態

// 目前頁碼
const currentPage = ref(1);
// 所有站點資料
const uBikeStops = ref([]);
// 目前的排序選項
const currentSort = ref('sno');
// 是否為降冪排序
const isSortDesc = ref(false);
// 一頁幾筆資料
const COUNT_OF_PAGE = 20;
// 搜尋文字
const searchText = ref('');

fetch('https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json')
  .then(res => res.text())
  .then(data => {
    uBikeStops.value = JSON.parse(data);
  });

const updateCurrentPage = (value) => { currentPage.value = value };
const updateSearchText = (value) => { searchText.value = value };

// 篩選後的站點資料
const filtedUbikeStops = computed(() => {
  return uBikeStops.value.length === 0
    ? []
    : uBikeStops.value.filter(d => d.sna.includes(searchText.value));
});

// 排序後的站點資料
const sortedUbikeStops = computed(() => {
  const filtedStops = [...filtedUbikeStops.value];

  return isSortDesc.value
    ? filtedStops.sort((a, b) => b[currentSort.value] - a[currentSort.value])
    : filtedStops.sort((a, b) => a[currentSort.value] - b[currentSort.value]);
});


// 分頁後的站點資料
const slicedUbikeStops = computed(() => {
  const start = (currentPage.value - 1) * COUNT_OF_PAGE;
  const end =
    start + COUNT_OF_PAGE <= sortedUbikeStops.value.length
      ? start + COUNT_OF_PAGE
      : sortedUbikeStops.value.length;
  return sortedUbikeStops.value.slice(start, end);
});

// 監聽搜尋文字，若有變動則將頁碼回歸第一頁
watch(searchText, () => {
  currentPage.value = 1
});
</script>

<template>
  <div class="app">
    <Search :uBikeStops="uBikeStops" @dispatchSearchText="updateSearchText"/>
    <uTable :slicedUbikeStops="slicedUbikeStops" 
            :searchText="searchText" 
            @dispatchCurrentSort="currentSort = $event"
            @dispatchIsSortDesc="isSortDesc = $event"/>
  </div>

  <!-- 頁籤 -->
  <Pagination :filtedUbikeStops="filtedUbikeStops" @dispatchPage="updateCurrentPage" :currentPage="currentPage"/>
</template>

<style lang="scss" scoped>
.app {
  padding: 1rem;
}
.pointer {
  cursor: pointer;
}
.pagination {
  display: flex;
  justify-content: center;
}
@media (max-width: 768px) {
  .sno {
    max-width: 50px; word-wrap: break-word;
  }
  .table td, .table th {
    padding: .5rem .25rem;
  }
}
</style>