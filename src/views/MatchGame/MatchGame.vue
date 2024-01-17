<script setup>
import { ref } from 'vue';
import card from './component/card.vue'

// 試完成以下功能：
//  1. 點擊卡片，卡片會翻開 (已完成)
//  2. 點擊兩張不同的卡片，卡片會翻回去 (已完成)
//  3. 點擊兩張相同的卡片，卡片會消失 (已完成)
//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲
//  5. 將卡片獨立抽出為 Card.vue 元件 (已完成)

const cards = ref([]);
const openedCard = ref([]);
const matchedCard = ref([])


// 遊戲初始化，洗牌
const gameInit = () => {
  const numArr = [...new Array(16).keys()].map(i => ++i);
  numArr.sort(() => Math.random() - 0.5);
  cards.value = numArr.map(d => (d % 8) + 1);
  openedCard.value = [];
  matchedCard.value = [];
}


const clickHandler = (idx) => {    
  if (openedCard.value.length < 2 && !openedCard.value.includes(idx)) {
    openedCard.value.push(idx);
  }
  
  if (openedCard.value.length == 2) {
    if (getCardsNum(openedCard.value[0]) != getCardsNum(openedCard.value[1])) {
      cleanOpenCard(500)
    } else {
        matchedCard.value = matchedCard.value.concat(openedCard.value)
        cleanOpenCard(0)
    }
  } else {
    cleanOpenCard(3000)
  }
}



const cleanOpenCard = (ms) => {
  window.setTimeout(() => {
    openedCard.value = [];
  }, ms); 
}

const getCardsNum = (idx) => {
  return cards.value[idx]
}

</script>

<template>
  <div class="bg-emerald-900 h-[100vh] w-full top-0 left-0 z-10 absolute">

    <div class="my-10 text-white text-center ">
      <div class="mb-8 text-5xl">五倍對對碰</div>
      <button 
        @click="gameInit"
        class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">開始</button>
    </div>

    <div class="rounded-xl mx-auto border-4 mt-12 grid grid-flow-col p-10 w-[900px] gap-2 grid-rows-4">
      
      <card 
        v-for="(n, idx) in cards"
        @click="clickHandler(idx)"
        :n="n"
        :idx="idx"
        :cards="cards"
        :openedCard="openedCard"
        :matchedCard="matchedCard">
      </card>
      <div v-if="cards.length > 0 && cards.length == matchedCard.length">
        <p>恭喜破關，再來一局？</p>
        <button 
        @click="gameInit"
        class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">確認</button>
      </div>
    </div>
  </div>
</template>

<style scoped src="./MatchGame.css"></style>