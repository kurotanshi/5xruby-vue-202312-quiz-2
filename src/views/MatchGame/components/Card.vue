<script setup>

const props = defineProps({
    cards: Array,
    openedCard: Array,
    hiddenCard: Array
});

const emit = defineEmits('clickHandler');

</script>

<template>
    <div v-for="(n, idx) in props.cards" class="flip-card" :class="{
        'open': props.openedCard.includes(idx)
    }" @click="emit('clickHandler', idx, n)">
        <div class="flip-card-inner" v-show="!props.hiddenCard.includes(n)" v-if="cards[idx] > 0">
            <div class="flip-card-front">{{ n }}</div>
            <div class="flip-card-back">
                <img :src="`./img/cat-0${n}.jpg`" alt="">
            </div>
        </div>
    </div>
</template>

<style src="../MatchGame.css" scoped></style>