
<script setup>
  import { ref } from "vue";

  // props 傳遞 function 方式
  const props = defineProps ({
    receive: Function
  })

  // 搜尋文字
  const searchText = ref('');

</script>

<template>
  <!-- emit 傳遞方式 -->
  <!-- <div>
    <p class="mb-0">
      站點名稱搜尋: <input class="border" type="text" v-model="searchText" @change="$emit('passUpSearchText', searchText)">
    </p>
  </div> -->

  <!-- props 傳遞 function 方式 -->
  <div>
    <p class="mb-0">
      站點名稱搜尋: <input class="border" type="text" v-model="searchText" @change="receive(searchText)">
    </p>
  </div>
</template>