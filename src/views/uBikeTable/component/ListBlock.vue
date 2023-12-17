<script setup>
  import { ref, computed, watch } from "vue";
  import ListItem from './ListItem.vue'

  const props = defineProps({
    uBikeStopsAfterFiltered: Array,
    searchText: String,
  })
  
  // ============ 資料接收 ============ //
  // 等待後端返回的值，有值後計算 sortedUbikeStops 屬性
  const receivedData = ref([]);
  watch(() => props.uBikeStopsAfterFiltered, (value) => {
    console.log(value, '----watch');
    receivedData.value = value;
  })


  // ============ 排序 ============ //
  // 目前的排序選項
  const currentSort = ref('sno');

  // 是否為降冪排序
  const isSortDesc = ref(false);

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
    const filtedStops = [...receivedData.value];

    return isSortDesc.value
      ? filtedStops.sort((a, b) => b[currentSort.value] - a[currentSort.value])
      : filtedStops.sort((a, b) => a[currentSort.value] - b[currentSort.value]);
  });

  
  
</script>

<template>
  <!-- {{ uBikeStopsAfterFiltered }} -->
  <table class="table table-striped">
    <!-- {{ sortedUbikeStops[0].sno }} -->
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
          v-for="s in sortedUbikeStops" :key="s.sno"
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
