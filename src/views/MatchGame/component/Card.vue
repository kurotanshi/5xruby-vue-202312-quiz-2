<script setup>
    import { defineProps, defineEmits } from 'vue';

    const props = defineProps({
        cards: Array,
        openedCards: Array,
        clearedCards: Array,
        cheatMode: Boolean,
    });
    const emits = defineEmits(['clickCard'])
</script>


<template>
    <div v-for="(n, idx) in cards" 
        class="flip-card transition ease-in-out hover:-translate-y-1 hover:scale-110" 
        :class="{
            'open': openedCards.includes(idx),
            'transition ease-in-out opacity-0 delay-1000': clearedCards.includes(idx)
        }" 
        @click="$emit('clickCard',{idx, n})">
        
        <div class="flip-card-inner" v-if="cards[idx] > 0">
            <div class="flip-card-front"></div>
            <div class="flip-card-back">
                <img :src="`./img/cat-0${n}.jpg`" alt="">
            </div>
        </div>

        <p :class="cheatMode? 'block': 'hidden'" >idx:{{ idx }}, n:{{ n }}</p>
    </div>
</template>
<!-- @click="clickHandler(idx, n)"  -->
<style scoped src="../MatchGame.css"></style>