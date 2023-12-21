<script setup>
  import { computed } from "vue";
  const props = defineProps({
    totalPageCount: Number,
    currentPage: Number,
    PAGINATION_MAX: Number
  });

  const totalPageCount = computed(() => props.totalPageCount);
  const currentPage = computed(() => props.currentPage);
  const emit = defineEmits(['setCurrentPageState']);

  // 分頁的位移，用來確保目前的頁碼固定出現在中間
  const pagerAddAmount = computed(() => {
    const tmp =
      totalPageCount.value <= props.PAGINATION_MAX
        ? 0
        : currentPage.value + 4 - pagerEnd.value;
    return tmp <= 0
      ? 0
      : totalPageCount.value - (props.PAGINATION_MAX + tmp) < 0
        ? totalPageCount.value - props.PAGINATION_MAX
        : tmp;
  });

  const setPage = v => emit('setCurrentPageState', v);
  
  // 分頁的尾端
  const pagerEnd = computed(() => {
    return totalPageCount.value <= props.PAGINATION_MAX
      ? totalPageCount.value
      : props.PAGINATION_MAX;
  });
</script>
<template>
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

<style scoped>
.pagination {
  display: flex;
  justify-content: center;
}
</style>