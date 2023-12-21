<script setup>
    import { ref, onMounted, watch } from 'vue'
    // 搜尋文字
    const childText = ref('');
    

    // 監聽搜尋文字，若有變動則將頁碼回歸第一頁
    // watch(searchText, () => {
    // currentPage.value = 1;
    // });
    const props = defineProps({
        searchText: String
    })
    const emit = defineEmits(['updateText']);
    const textHandler = () => {
        emit('updateText', childText.value);
    }
</script>
<template>
    <p>
        站點名稱搜尋: <input class="border" type="text" v-model="childText" @input="textHandler">
    </p>
</template>
<style>
  
</style>