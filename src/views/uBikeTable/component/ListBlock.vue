<script setup>
  import { ref, computed } from "vue";
  import ListItem from './ListItem.vue'

  const props = defineProps({
    filtedUbikeStops: Array,
    currentPage: Number,
    COUNT_OF_PAGE: Number,
    PAGINATION_MAX: Number,
    searchText: String,
  })

  // ============ 變數宣告 ============ //
  // 目前的排序選項
  const currentSort = ref('sno');
  // 是否為降冪排序
  const isSortDesc = ref(false);
  // 目前頁碼
  

  // ============ 排序 ============ //
  // 指定排序
  const setSort = sortType => {
    if (sortType === currentSort.value) {
      isSortDesc.value = !isSortDesc.value;
    } else {
      currentSort.value = sortType;
      isSortDesc.value = false;
    }
  };
  // 排序後的站點資料
  const sortedUbikeStops = computed(() => {
    const filtedStops = [...props.filtedUbikeStops];

    return isSortDesc.value
      ? filtedStops.sort((a, b) => b[currentSort.value] - a[currentSort.value])
      : filtedStops.sort((a, b) => a[currentSort.value] - b[currentSort.value]);
  });


  // ============ 分頁 ============ //
  // 分頁後的站點資料
  const slicedUbikeStops = computed(() => {
    const start = (props.currentPage - 1) * props.COUNT_OF_PAGE;
    const end =
      start + props.COUNT_OF_PAGE <= sortedUbikeStops.value.length
        ? start + props.COUNT_OF_PAGE
        : sortedUbikeStops.value.length;
    return sortedUbikeStops.value.slice(start, end);
  });

</script>

<template>
  <table class="table table-striped">
    <thead>
      <tr>
        <th class="cursor-pointer" @click="setSort('sno')">
          #
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
        <th @click="setSort('sbi')" class="cursor-pointer">
          目前可用車輛
          <span v-show="currentSort === 'sbi'">
            <i class="fa" :class="isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'" aria-hidden="true"></i>
          </span>
        </th>
        <th @click="setSort('tot')" class="cursor-pointer">
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
      <ListItem
          v-for="s in slicedUbikeStops" :key="s.sno"
          :sno="s.sno"
          :sna="s.sna"
          :sarea="s.sarea"
          :sbi="s.sbi"
          :tot="s.tot"
          :mday="s.mday"
          :searchText="searchText"
      />
    </tbody>
  </table>
</template>
