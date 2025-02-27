<script>
export default {
    props: {
        userCount: Number,
        computerCount: Number
    }
}
</script>

<template>
    <section class="score">
        Player <span>{{ this.userCount }}</span> x <span>{{ this.computerCount }}</span> Computer
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