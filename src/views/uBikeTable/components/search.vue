<script setup>
import { ref } from 'vue';



// 搜尋文字
const searchText = ref('');

</script>

<template>
  <div class="app">
    <p>
      站點名稱搜尋: <input class="border" type="text" v-model="searchText" @input="$emit('dispatchSearchText', searchText)">
    </p>
  </div>

  
</template>