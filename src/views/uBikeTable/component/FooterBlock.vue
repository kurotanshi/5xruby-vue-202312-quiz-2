<script setup>
  import { ref, computed, watch } from "vue"; 
  const props = defineProps({
    currentPage:Number,
    PAGINATION_MAX: Number,
    totalPageCount: Number,
    pagerEnd: Number,
    pagerAddAmount: Number
  })
  //確認
  watch(() => props.totalPageCount, (value) => {

    // 分頁的位移，用來確保目前的頁碼固定出現在中間 
    const pagerAddAmount = computed(() => {
      const tmp =
        totalPageCount.value <= PAGINATION_MAX
          ? 0
          : currentPage.value + 4 - pagerEnd.value;
      return tmp <= 0
        ? 0
        : totalPageCount.value - (PAGINATION_MAX + tmp) < 0
          ? totalPageCount.value - PAGINATION_MAX
          : tmp;
    });
  })
  
  // 換頁 (在FooterBlock)
  const setPage = page => {
    if (page < 1 || page > totalPageCount.value) {
      return;
    }
    currentPage.value = page;
  };
</script>

<template>
  <h2>prop test</h2>
  <h2>{{ currentPage }}</h2>
  <h2>{{ PAGINATION_MAX }}</h2>
  <h2>{{ pagerEnd }}</h2>
  <h2>{{ totalPageCount }}</h2>
  <h2>{{ pagerAddAmount }}</h2>

  <nav v-if="pagerEnd > 0">
    <ul class="pagination">

      <li @click.prevent="setPage(1)" class="page-item">
        <a class="page-link" href>第一頁</a>
      </li>
      <li @click.prevent="setPage(currentPage - 1)" class="page-item">
        <a class="page-link" href>&lt;</a>
      </li>

      <li v-for="i in pagerEnd" :class="{ active: i + pagerAddAmount === currentPage }" :key="i"
        @click.prevent="setPage(i + pagerAddAmount)" class="page-item">
        <a class="page-link" href>{{ i + pagerAddAmount }}</a>
      </li>

      <li @click.prevent="setPage(currentPage + 1)" class="page-item">
        <a class="page-link" href>&gt;</a>
      </li>
      <li @click.prevent="setPage(totalPageCount)" class="page-item">
        <a class="page-link" href>最末頁</a>
      </li>
    </ul>
  </nav>
</template>

<style lang="scss">
  .pagination {
    display: flex;
    justify-content: center;
  }
</style>