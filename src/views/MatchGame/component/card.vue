<script setup>
  import { ref } from 'vue';

  const props = defineProps({
    n: Number, 
    idx: Number,
    cards: Array,
    openedCard: Array,
    matchedCard: Array
  })

  const cards = ref(props.cards);
  const index = ref(props.idx);
  const number = ref(props.n);

  // const emit = defineEmits(['dispatchOpendCard']);
  // @click="$emit('dispatchOpendCard', index)"
</script>


<template>
  <div class="flip-card" 
       :class="{ 'open': props.openedCard.includes(index) }"
       v-show="cards.length != props.matchedCard.length">
       <div class="flip-card-inner" v-if="cards[index] > 0"  v-show="!props.matchedCard.includes(index)">
        <div class="flip-card-front"></div>
      <div class="flip-card-back">
        <img :src="`./img/cat-0${number}.jpg`" alt="">
      </div>
    </div>
  </div>
</template>

<style scoped src="../MatchGame.css"></style>