<script setup>
  import SearchBlock from './component/SearchBlock.vue';
  import ListBlock from './component/ListBlock.vue';
  import FooterBlock from './component/FooterBlock.vue'

  import { ref, computed, watch } from 'vue';
  import "bootstrap/dist/css/bootstrap.css";
  import '@fortawesome/fontawesome-free/css/all.css';

  // 修改這份 YouBike 即時資訊表：
  // 1. 將搜尋的部分拆出來變成子元件 `uBikeTable/components/search.vue`
  // 2. 將表格的部分拆出來變成子元件 `uBikeTable/components/uBikeTable.vue`
  // 3. 將分頁的部分拆出來變成子元件 `uBikeTable/components/pagination.vue`
  // 4. 再將它們組合起來

  // 所有站點資料
  const uBikeStops = ref([]);
  // 搜尋文字
  const searchText = ref('');
  // 目前頁碼
  const currentPage = ref(1);
  // 一頁幾筆資料
  const COUNT_OF_PAGE = 10;
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

  // 篩選後的站點資料 (父元件)
  const filtedUbikeStops = computed(() => {
    return uBikeStops.value.length === 0
      ? []
      : uBikeStops.value.filter(d => d.sna.includes(searchText.value));
  });

  // 總頁數
  const totalPageCount = computed(() => {
    return Math.ceil(filtedUbikeStops.value.length / COUNT_OF_PAGE);
  });
  

  // ============ emit ============ //
  // 接收 SearchBlock 傳上來的值
  const passUpSearchText = (value) => {
    searchText.value = value;
  }

  // 接收 FooterBlock 傳上來的頁籤值
  const setCurrentPageFromFooter = (value) => {
    if (value < 1 || value > totalPageCount.value) {
      return;
    }
    currentPage.value = value;
  }


  // ============ props 傳遞 function ============ //
  const receive = (value) => {
    searchText.value = value
  }
</script>

<template>
  <div class="app">
    <!-- emit 傳遞方式 -->
    <!-- <SearchBlock @passUpSearchText="passUpSearchText"/> -->

    <!-- props 傳遞 function 方式 -->
    <SearchBlock :receive="receive"/>

    <ListBlock 
        :filtedUbikeStops="filtedUbikeStops"
        :currentPage="currentPage"
        :COUNT_OF_PAGE="COUNT_OF_PAGE"
        :PAGINATION_MAX="PAGINATION_MAX"
        :searchText="searchText"/>

    <FooterBlock
        :filtedUbikeStops="filtedUbikeStops"
        :totalPageCount="totalPageCount"
        :currentPage="currentPage"
        :COUNT_OF_PAGE="COUNT_OF_PAGE"
        :PAGINATION_MAX="PAGINATION_MAX"
        @fromFooterCurrentPage="setCurrentPageFromFooter"/>
        
  </div>
  <!-- 頁籤 -->
</template>

<style lang="scss">
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