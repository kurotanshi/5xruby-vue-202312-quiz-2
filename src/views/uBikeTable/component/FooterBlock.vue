<script setup>
  import { computed } from "vue"; 
  const props = defineProps({
    filtedUbikeStops: Array,
    totalPageCount: Number,
    currentPage: Number,
    COUNT_OF_PAGE: Number,
    PAGINATION_MAX: Number
  })

  const emits = defineEmits(["fromFooterCurrentPage"]);
  
  // 分頁的尾端
  const pagerEnd = computed(() => {
    return props.totalPageCount <= props.PAGINATION_MAX
      ? props.totalPageCount
      : props.PAGINATION_MAX;
  });

  // 分頁的位移，用來確保目前的頁碼固定出現在中間 (在FooterBlock)
  const pagerAddAmount = computed(() => {
    const tmp =
      props.totalPageCount <= props.PAGINATION_MAX
        ? 0
        : props.currentPage + 4 - pagerEnd.value;
    return tmp <= 0
      ? 0
      : props.totalPageCount - (props.PAGINATION_MAX + tmp) < 0
        ? props.totalPageCount - props.PAGINATION_MAX
        : tmp;
  });

</script>

<template>
  <nav v-if="pagerEnd > 0">
    <ul class="pagination">
      <!-- 第一頁 -->
      <li @click.prevent="$emit('fromFooterCurrentPage',1)" class="page-item">
        <a class="page-link" href>第一頁</a>
      </li>
      <!-- 上一頁icon -->
      <li @click.prevent="$emit('fromFooterCurrentPage', currentPage - 1)" class="page-item">
        <a class="page-link" href>&lt;</a>
      </li>
      <!-- 頁碼 -->
      <li v-for="i in pagerEnd" :class="{ active: i + pagerAddAmount === currentPage }" :key="i"
        @click.prevent="$emit('fromFooterCurrentPage', i + pagerAddAmount)" class="page-item">
        <a class="page-link" href>{{ i + pagerAddAmount }}</a>
      </li>
      <!-- 下一頁 icon -->
      <li @click.prevent="$emit('fromFooterCurrentPage', currentPage + 1)" class="page-item">
        <a class="page-link" href>&gt;</a>
      </li>
      <!-- 最末頁 -->
      <li @click.prevent="$emit('fromFooterCurrentPage', totalPageCount)" class="page-item">
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