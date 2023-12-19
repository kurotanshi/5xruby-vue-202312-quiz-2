<script setup>

  const props = defineProps({
    sno: String,
    sna: String,
    sarea: String,
    sbi: Number,
    tot: Number,
    mday: String,
    searchText: String,
  })

  // ============ 關鍵字強調 ============ //
  // 關鍵字 Highlight
  const keywordsHighlight = (text, keyword) => {
    if (keyword === '') return text;
    const reg = new RegExp(keyword, 'gi');
    return text.replace(reg, `<span style="color: red;">${keyword}</span>`);
  };
</script>

<template>
  <tr>
    <td>{{ sno }}</td>
    <td v-html="keywordsHighlight(sna, searchText)"></td>
    <td>{{ sarea }}</td>
    <td>{{ sbi }}</td>
    <td>{{ tot }}</td>
    <td>{{ mday }}</td>
  </tr>
  
</template>

