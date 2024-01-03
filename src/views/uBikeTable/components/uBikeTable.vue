<script setup>
import { ref } from 'vue';

// 目前的排序選項
const currentSort = ref('sno');
// 是否為降冪排序
const isSortDesc = ref(false);

// const emit = defineEmits(['pageUpdate']);
// const func = () => {
//   emit('pageUpdate');
// };

const props = defineProps({
  slicedUbikeStops: Array,
  searchText: String
})

const emit = defineEmits(['dispatchCurrentSort', 'dispatchIsSortDesc']);

// 指定排序
const setSort = sortType => {
  if (sortType === currentSort.value) {
    isSortDesc.value = !isSortDesc.value;
  } else {
    currentSort.value = sortType;
    isSortDesc.value = false;
  }

  emit('dispatchCurrentSort', currentSort.value)
  emit('dispatchIsSortDesc', isSortDesc.value)
};

// 關鍵字 Highlight
const keywordsHighlight = (text, keyword) => {
  if(keyword === '') return text;
  const reg = new RegExp(keyword, 'gi');
  return text.replace(reg, `<span style="color: red;">${keyword}</span>`);
};



</script>

<template>
  <div class="app">
    <table class="table table-striped">
      <thead>
        <tr>
          <th @click="setSort('sno')">
            <span v-show="currentSort === 'sno'">
              <i class="fa" :class="isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'" aria-hidden="true"></i>
            </span>
          </th>
          <th>
            場站名稱
          </th>
          <th>
            場站區域
          </th>
          <th @click="setSort('sbi')" class="pointer">
            目前可用車輛
            <span v-show="currentSort === 'sbi'">
              <i class="fa" :class="isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'" aria-hidden="true"></i>
            </span>
          </th>
          <th @click="setSort('tot')" class="pointer">
            總停車格
            <span v-show="currentSort === 'tot'">
              <i class="fa" :class="isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'" aria-hidden="true"></i>
            </span>
          </th>
          <th>
            資料更新時間
          </th>
        </tr>
      </thead>
      <tbody>
        <!-- 替換成 slicedUbikeStops -->
        <tr v-for="s in props.slicedUbikeStops" :key="s.sno"  >
          <td>{{ s.sno }}</td>
          <td v-html="keywordsHighlight(s.sna, props.searchText)"></td>
          <td>{{ s.sarea }}</td>
          <td>{{ s.sbi }}</td>
          <td>{{ s.tot }}</td>
          <td>{{ (s.mday) }}</td>
        </tr>
      </tbody>
    </table>
  </div>

  <!-- 頁籤 -->
  
</template>