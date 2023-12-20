<script setup>
import { computed, defineProps, defineEmits } from "vue";

const props = defineProps({
  internalSearchText: String
});

const emits = defineEmits(['updateSearchText']);

const setSearch = computed({
  get: () => props.internalSearchText,
  set: v => emits('setSearchState', v)
});
</script>

<template>
  <p>
    站點名稱搜尋: <input class="border" type="text" v-model="setSearch">
  </p>
</template>
