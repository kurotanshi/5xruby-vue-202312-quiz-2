<script setup>
  import { ref, onMounted, computed } from 'vue'


    const props = defineProps({
        sno: String,
        sna: String,
        sarea: String,
        sbi: Number,
        tot: Number,
        mday:String,
        searchText: String
    })
    // 篩選後的站點資料
    const filtedUbikeStops = computed(() => {
        return uBikeStops.value.length === 0
            ? []
            : uBikeStops.value.filter(d => d.sna.includes(searchText.value));
    });
    // 關鍵字 Highlight
    const keywordsHighlight = (text, keyword) => {
        if(keyword === '') return text;
        const reg = new RegExp(keyword, 'gi');
        return text.replace(reg, `<span style="color: red;">${keyword}</span>`);
    };
</script>
<template>
    <tr>
        <td>{{ sno }}</td>
        <td v-html="keywordsHighlight(sna, searchText)"></td>
        <!-- <td>{{ sna }}</td> -->
        <td>{{ sarea }}</td>
        <td>{{ sbi }}</td>
        <td>{{ tot }}</td>
        <td>{{ (mday) }}</td>
    </tr>
</template>
<style>
  
</style>