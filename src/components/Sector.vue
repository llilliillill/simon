<template>
    <div 
        :id="id" 
        @click="handlerClick" 
        :class="[ 
            active === Number(id) ? 'sector active' : 'sector' 
        ]"
    ></div>
</template>

<script lang="ts" setup>
    defineProps<{
        id: string,
        handlerClick: Function,
        active: number | null
    }>()
</script>

<style scoped>
    .sector {
        width: 175px;
        height: 175px;
        background-color: gray;
        cursor: pointer;
    }

    .sector:nth-child(1) { 
        background: yellow; 
        border-bottom: 10px solid rgb(63, 63, 63);
        border-right: 10px solid rgb(63, 63, 63);
    }

    .sector:nth-child(2) { 
        background: red; 
        border-bottom: 10px solid rgb(63, 63, 63);
        border-left: 10px solid rgb(63, 63, 63);
    }

    .sector:nth-child(3) { 
        background: green; 
        border-top: 10px solid rgb(63, 63, 63);
        border-right: 10px solid rgb(63, 63, 63);
    }

    .sector:nth-child(4) { 
        background: blue; 
        border-top: 10px solid rgb(63, 63, 63);
        border-left: 10px solid rgb(63, 63, 63);
    }

    .active {
        animation-name: blinker;
        animation-iteration-count: infinite;
        animation-timing-function: cubic-bezier(1.0,2.0,0,1.0);
        animation-duration: 1s;
        -webkit-animation-name: blinker;
        -webkit-animation-iteration-count: infinite;
        -webkit-animation-timing-function: cubic-bezier(1.0,2.0,0,1.0);
        -webkit-animation-duration: 1s;
    }

    @keyframes blinker {
        from { opacity: 1.0; }
        to { opacity: 0.0; }
    }

    @-webkit-keyframes blinker {
        from { opacity: 1.0; }
        to { opacity: 0.0; }
    }
</style>