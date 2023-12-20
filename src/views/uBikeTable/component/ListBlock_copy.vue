<script setup>
  import { ref, computed, watch, defineProps, defineEmits } from "vue";
  import ListItem from './ListItem.vue'

  const props = defineProps({
    slicedUbikeStops: Array,
    currentSort: String,
    isSortDesc: Boolean,

    searchText: String, //ListItem 用
  })
  const emits = defineEmits(["passUpSortSet"]) 
  

</script>

<template>
  <table class="table table-striped">
    <thead>
      <tr>
        <th class="cursor-pointer" @click="$emit('passUpSortSet', 'sno')">
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
        <th @click="$emit('passUpSortSet', 'sbi')" class="cursor-pointer">
          目前可用車輛
          <span v-show="currentSort === 'sbi'">
            <i class="fa" :class="isSortDesc ? 'fa-sort-desc' : 'fa-sort-asc'" aria-hidden="true"></i>
          </span>
        </th>
        <th @click="$emit('passUpSortSet', 'tot')" class="cursor-pointer">
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
