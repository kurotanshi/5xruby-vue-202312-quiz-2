<script setup>
// import { ref, onMounted } from 'vue'
const props = defineProps({
    totalPageCount: Number,
    pageEnd: Number, 
    pageAddAmount: Number,
    currentPage: Number,
    setPage: Function,
})

</script>
<template>
    <nav v-if="pageEnd > 0">
        <ul class="pagination">

        <li @click.prevent="setPage(1)" class="page-item">
            <a class="page-link" href>第一頁</a>
        </li>
        <li @click.prevent="setPage(currentPage - 1)" class="page-item">
            <a class="page-link" href>&lt;</a>
        </li>

        <li v-for="i in pageEnd" :class="{ active: i + pageAddAmount === currentPage }" :key="i"
            @click.prevent="setPage(i + pageAddAmount)" class="page-item">
            <a class="page-link" href>{{ i + pageAddAmount }}</a>
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
<style>
  nav {
    width: fit-content;
    margin: auto;
  }
</style>