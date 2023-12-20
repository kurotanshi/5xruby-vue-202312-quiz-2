
<script setup>
  import { ref } from "vue";

  // 搜尋文字
  const searchText = ref('');

</script>

<template>
  <div>
    <p class="mb-0">
      站點名稱搜尋: <input class="border" type="text" v-model="searchText" @change="$emit('passUpSearchText', searchText)">
    </p>
  </div>
</template>