<script setup>
import { ref, computed } from 'vue';

const emit = defineEmits(['dispatchPage']);

// 目前頁碼
const currentPage = ref(1);
// 一頁幾筆資料
const COUNT_OF_PAGE = 20;
// 頁碼最多顯示幾頁
const PAGINATION_MAX = 10;


// 總頁數
const totalPageCount = computed(() => {
  return Math.ceil(props.filtedUbikeStops.length / COUNT_OF_PAGE);
});

// 分頁的尾端
const pagerEnd = computed(() => {
  return totalPageCount.value <= PAGINATION_MAX
    ? totalPageCount.value
    : PAGINATION_MAX;
});

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

// 換頁
const setPage = page => {
  if (page < 1 || page > totalPageCount.value) {
    return;
  }
  currentPage.value = page;
  emit('dispatchPage', currentPage.value)
};

const props = defineProps({
  filtedUbikeStops: Array,
  currentPage: Number
})

</script>



<template>
  

  <!-- 頁籤 -->
  <nav v-if="pagerEnd  > 0">
    <ul class="pagination">

      <li @click.prevent="setPage(1)" class="page-item">
        <a class="page-link" href>第一頁</a>
      </li>
      <li @click.prevent="setPage(currentPage - 1)" class="page-item">
        <a class="page-link" href>&lt;</a>
      </li>

      <li v-for="i in pagerEnd" :class="{ active: i + pagerAddAmount === currentPage }"  :key="i"
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