<template>
    <section class="center">
        <Logo />
        <div class="body">
            <div class="block-level">
                <div class="wrapper">
                    <span class="text">рек. </span>
                    <span class="title">{{ record }}</span>
                </div>
                <div class="wrapper">
                    <span class="text">ур. </span>
                    <span class="title">{{ level }}</span>
                </div>
            </div>
            <div class="block-start">
                <button class="start" @click="start()"></button>
                <span class="text">играть</span>
            </div>
        </div>
    </section>
</template>

<script lang="ts" setup>
    import Logo from '@/components/Logo.vue'

    defineProps<{
        record: number,
        level: number,
        start: Function
    }>()
</script>

<style scoped>
    .center {
        background-color: rgb(63, 63, 63);
        border-radius: 50%;
        width: 150px;
        height: 150px;
        position: absolute;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        cursor: default;
    }

    .body {
        width: 120px;
        height: 35px;
        display: flex;
        justify-content: space-around;
        align-items: center;
    }

    .start {
        width: 20px;
        height: 20px;
        background-color: red;
        border: none;
        outline: none;
        border-radius: 50%;
        margin: 0 auto;
        cursor: pointer;
    }

    .block-level,
    .block-start {
        display: flex;
        flex-direction: column;
    }

    .block-start {
        color: white;
    }

    .wrapper {
        display: flex;
        color: white;
    }

    .title {
        margin-left: 5px;
    }

    .text {
        flex: 1;
    }
</style>