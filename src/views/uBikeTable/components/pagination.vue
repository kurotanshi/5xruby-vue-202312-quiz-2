<script setup>
import { ref, computed, watch } from 'vue';


const props = defineProps({
    currentPage: Number,
    pagerEnd: Number,
    pagerAddAmount: Number,
    totalPageCount: Number,
});

const emitSetPage = defineEmits(['setPage']);


</script>

<template>
    <nav v-if="pagerEnd > 0">
        <ul class="pagination">

            <li @click.prevent="emitSetPage('setPage', 1)" class="page-item">
                <a class="page-link" href>第一頁</a>
            </li>
            <li @click.prevent="emitSetPage('setPage', currentPage - 1)" class="page-item">
                <a class="page-link" href>&lt;</a>
            </li>

            <li v-for="i in pagerEnd" :class="{ active: i + pagerAddAmount === props.currentPage }" :key="i"
                @click.prevent="emitSetPage('setPage', i + pagerAddAmount)" class="page-item">
                <a class="page-link" href>{{ i + pagerAddAmount }}</a>
            </li>

            <li @click.prevent="emitSetPage('setPage', props.currentPage + 1)" class="page-item">
                <a class="page-link" href>&gt;</a>
            </li>
            <li @click.prevent="emitSetPage('setPage', props.totalPageCount)" class="page-item">
                <a class="page-link" href>最末頁</a>
            </li>
        </ul>
    </nav>
</template>

<style lang="scss" scoped></style>