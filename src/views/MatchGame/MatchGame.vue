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
	const openedCards = ref([]);	//紀錄打開的卡片
	const clearedCards = ref([]);	//紀錄對對碰成功的卡片


	// 遊戲初始化，洗牌
	const gameInit = () => {
		const numArr = [...new Array(16).keys()].map(i => ++i);
		console.log(numArr);
		numArr.sort(() => Math.random() - 0.5);
		console.log(numArr);
		cards.value = numArr.map(d => (d % 8) + 1);

		openedCards.value = [];
		clearedCards.value = [];
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
			<!-- 插入元件 -->
			<Card 
				:cards="cards"
				:openedCards="openedCards"
				:clearedCards="clearedCards"
				
				/>

		</div>
	</div>
</template>

<!-- <style scoped src="./MatchGame.css"></style> -->