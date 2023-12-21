<script setup>
import { ref, computed, watch } from 'vue';
import "bootstrap/dist/css/bootstrap.css";
import '@fortawesome/fontawesome-free/css/all.css';
import search from './components/search.vue'
import uBikeTable from './components/uBikeTable.vue'
import pagination from "./components/pagination.vue";

// 所有站點資料
const uBikeStops = ref([]);
// 搜尋文字
const searchText = ref('');
// 目前頁碼
const currentPage = ref(1);
// 一頁幾筆資料
const COUNT_OF_PAGE = 20;
// 頁碼最多顯示幾頁
const PAGINATION_MAX = 10;

fetch('https://tcgbusfs.blob.core.windows.net/dotapp/youbike/v2/youbike_immediate.json')
  .then(res => res.text())
  .then(data => {
    uBikeStops.value = JSON.parse(data);
  });

// 監聽搜尋文字，若有變動則將頁碼回歸第一頁
watch(searchText, () => {
  currentPage.value = 1;
});

const handleSearchState = (t) => {
  searchText.value = t;
};

// 篩選後的站點資料
const filtedUbikeStops = computed(() => {
  return uBikeStops.value.length === 0
    ? []
    : uBikeStops.value.filter(d => d.sna.includes(searchText.value));
});

// 分頁後的站點資料
const slicedUbikeStops = computed(() => {
  const start = (currentPage.value - 1) * COUNT_OF_PAGE;
  const end =
    start + COUNT_OF_PAGE <= filtedUbikeStops.value.length
      ? start + COUNT_OF_PAGE
      : filtedUbikeStops.value.length;
  return filtedUbikeStops.value.slice(start, end);
});

// 總頁數
const totalPageCount = computed(() => {
  return Math.ceil(filtedUbikeStops.value.length / COUNT_OF_PAGE);
});

 // 換頁
 const setPage = page => {
    if (page < 1 || page > totalPageCount.value) {
      return;
    }
    currentPage.value = page;
  };

</script>

<template>
  <div class="app">
    <search :internalSearchText="searchText" @setSearchState="handleSearchState"></search>
    <uBikeTable
      :internalSearchText="searchText"
      :slicedUbikeStops="slicedUbikeStops"
    />
  </div>
  <!-- 頁籤 -->
  <pagination
    :totalPageCount="totalPageCount"
    :currentPage="currentPage"
    :PAGINATION_MAX="PAGINATION_MAX"
    @setCurrentPageState="setPage"
  />
</template>

<style lang="scss" scoped>
.app {
  padding: 1rem;
}
.pointer {
  cursor: pointer;
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