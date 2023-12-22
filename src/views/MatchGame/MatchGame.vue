<script setup>
	import { ref } from 'vue';
	import Card from './component/Card.vue';

	// 試完成以下功能：
	//  1. 點擊卡片，卡片會翻開 (已完成)
	//  2. 點擊兩張不同的卡片，卡片會翻回去 (done)
	//  3. 點擊兩張相同的卡片，卡片會消失 (done)
	//  4. 當所有卡片都消失時，顯示「恭喜破關，再來一局？」的對話框，按下確定後重置遊戲 (done)
	//  5. 將卡片獨立抽出為 Card.vue 元件

	const cards = ref([]);          // 紀錄開局卡片
	const openedCards = ref([]);	// 紀錄打開的卡片
	const clearedCards = ref([]);	// 紀錄對對碰成功的卡片
	const compareCards = ref([]);	// 卡片比較區
    const cheatMode = ref(false);    // 作弊開關

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

    // 點擊卡片判斷
	const clickHandler = (value) => {
        openedCards.value.push(value.idx)
        compareCards.value.push(value.n)

        if (compareCards.value[1] != null) {
            if (compareCards.value[0] == compareCards.value[1]) {
                //比對正確 goes here
                compareCards.value = [];
                clearedCards.value = [...openedCards.value];

                if (clearedCards.value.length === cards.value.length) {
                    //比對正確 goes here
                    console.log("finished~~~~~~~");
                    window.setTimeout(() => {
                        const result = window.confirm('讚喔！恭喜破關，再來一局？');
                        if (result) {
                            gameInit();
                        }
                    }, 1000);
                }
            } else {
                //比對失敗 goes here
                window.setTimeout(() => {
                    openedCards.value.splice(-2);
                }, 1000);
                compareCards.value = [];
            }
        }
    }

    // 作弊模式開關
    const toggleCheatMode = () => {
        cheatMode.value = !cheatMode.value;
    }
</script>

<template>
	<div class="bg-emerald-900 h-[100vh] w-full top-0 left-0 z-10 absolute">

		<div class="my-10 text-white text-center ">
		<div class="mb-8 text-5xl">五倍對對碰</div>
            <button 
                @click="gameInit"
                class="rounded font-bold bg-blue-500 mx-6 text-white py-2 px-4 hover:bg-blue-700">開始</button>
            <button 
                @click="toggleCheatMode"
                class="rounded font-bold mx-6 text-white py-2 px-4 hover:bg-white/20">{{ cheatMode ? '關閉作弊模式' : '開啟作弊模式' }}</button>
		</div>

		<div class="rounded-xl mx-auto border-4 mt-12 grid grid-flow-col p-10 w-[900px] gap-2 grid-rows-4">
			<!-- 插入元件 -->
			<Card 
				:cards="cards"
				:openedCards="openedCards"
				:clearedCards="clearedCards"
                :cheatMode="cheatMode"
				@clickCard="clickHandler"/>
		</div>
	</div>
</template>