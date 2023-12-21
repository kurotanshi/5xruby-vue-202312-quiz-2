<script setup>
const props = defineProps({
    passName: String
  });
</script>

<template>
    <p>
        站點名稱搜尋: <input class="border" type="text" :value="passName" 
    @input="$emit('update:pass-name', $event.target.value)">
    </p>
</template>

<style scoped></style>
