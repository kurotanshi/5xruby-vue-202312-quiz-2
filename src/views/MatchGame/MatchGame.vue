<script setup>
	import { ref } from 'vue';
	import Card from './component/Card.vue';

	// 試完成以下功能：
	//  1. 點擊卡片，卡片會翻開 (已完成)
	//  2. 點擊兩張不同的卡片，卡片會翻回去 (done)
	//  3. 點擊兩張相同的卡片，卡片會消失 (done)
	//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲 (done)
	//  5. 將卡片獨立抽出為 Card.vue 元件

	const cards = ref([]);          //紀錄開局卡片
	const openedCard = ref([]);		//紀錄打開的卡片
	const compareCards = ref([]);	//卡片比較區
	const clearedCards = ref([]);	//紀錄對對碰成功的卡片

	// 遊戲初始化，洗牌
	const gameInit = () => {
		const numArr = [...new Array(16).keys()].map(i => ++i);
		console.log(numArr);
		numArr.sort(() => Math.random() - 0.5);
		console.log(numArr);
		cards.value = numArr.map(d => (d % 8) + 1);
		openedCard.value = [];
		clearedCards.value = [];
	}
	
	const clickHandler = (idx, n) => {   

		openedCard.value.push(idx)
		compareCards.value.push(n)

		if (compareCards.value[1] != null) {
			if (compareCards.value[0] == compareCards.value[1]) {
				//比對正確 goes here
				compareCards.value = [];
				clearedCards.value = [...openedCard.value];

				if (clearedCards.value.length === cards.value.length) {
					
					window.setTimeout(() => {
						const result = window.confirm('好樣的！恭喜破關，再來一局？');
						if (result) {
							gameInit();
						}
					}, 1000);
				}
			} else {
				//比對失敗 goes here
				window.setTimeout(() => {
					openedCard.value.splice(-2);
				}, 1000);
				compareCards.value = [];
			}
		}
		
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
		
		<div 
			v-for="(n, idx) in cards"
			class="flip-card transition ease-in-out hover:-translate-y-1 hover:scale-110"
			:class="{
				'open': openedCard.includes(idx),
				'transition ease-in-out opacity-0 delay-1000': clearedCards.includes(idx)
			}"
			@click="clickHandler(idx, n)">
			idx:{{ idx }}  n:{{ n }}
			<div class="flip-card-inner" v-if="cards[idx] > 0">
			<div class="flip-card-front"></div>
			<div class="flip-card-back">
				<img :src="`./img/cat-0${n}.jpg`" alt="">
			</div>
			</div>
		</div>

		</div>
	</div>
</template>

<style scoped src="./MatchGame.css"></style>