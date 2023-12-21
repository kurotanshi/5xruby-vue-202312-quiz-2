<script setup>
    import { ref } from 'vue';

    const props = defineProps({
        cards: Array,
        openedCards: Array,
        clearedCards: Array,
    });
    
    props.clearedCards.push(100)
    // ============ 變數宣告 ============ //
    const compareCards = ref([]);	//卡片比較區
    console.log('props.cards.value', props.cards);

    console.log('props.clearedCards', props.clearedCards);
    // ============ 點擊事件 ============ //
    const clickHandler = (idx, n) => {
        

        props.openedCards.push(idx)
        compareCards.value.push(n)

        if (compareCards.value[1] != null) {
            if (compareCards.value[0] == compareCards.value[1]) {
                //比對正確 goes here
                compareCards.value = [];
                props.clearedCards = [...props.openedCards];

                if (props.clearedCards.length === props.cards.length) {
                    console.log("finished~~~~~~~");
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
                    props.openedCards.splice(-2);
                }, 1000);
                compareCards.value = [];
            }
            // props.clearedCards.push(...props.openedCards);
        }
    }

</script>


<template>
    <div v-for="(n, idx) in cards" 
        class="flip-card transition ease-in-out hover:-translate-y-1 hover:scale-110" 
        :class="{
            'open': openedCards.includes(idx),
            'transition ease-in-out opacity-0 delay-1000': clearedCards.includes(idx)
        }" 
        @click="clickHandler(idx, n)">
        idx:{{ idx }} n:{{ n }}
        <div class="flip-card-inner" v-if="cards[idx] > 0">
            <div class="flip-card-front"></div>
            <div class="flip-card-back">
                <img :src="`./img/cat-0${n}.jpg`" alt="">
            </div>
        </div>
    </div>
</template>

<style scoped src="../MatchGame.css"></style>