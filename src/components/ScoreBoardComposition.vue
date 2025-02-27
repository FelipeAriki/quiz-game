<script setup>
import { defineProps } from 'vue';

const props = defineProps({
    userCount: {typeof: Number, required: false},
    computerCount: {type: Number, required: false}
});
</script>

<template>
    <section class="score">
        Player <span>{{ props.userCount }}</span> x <span>{{ props.computerCount }}</span> Computer
    </section>
</template>

<style lang="scss">
section.score {
    border-bottom: 1px solid black;
    padding: 24px;
    font-size: 18px;

    span{
        padding: 8px;
        font-weight: bold;
        border: 1px solid black;
    }
}

</style>