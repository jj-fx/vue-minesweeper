<template>
    <div id="app">
        <h1 class="build-code">build : a_02</h1>
        <!--<HelloWorld msg="Welcome to Your Vue.js App"/>-->
        <Minesweeper/>
    </div>
</template>

<script>
    import Minesweeper from "./components/Minesweeper.vue";

    export default {
        name: 'App',
        components: {
            //HelloWorld,
            Minesweeper,
        },
    }
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@600&display=swap');

    #app {
        /*font-family: Avenir, Helvetica, Arial, sans-serif;*/
        font-family: 'Orbitron', sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #404040;
        margin-top: 60px;
        background-color: #3a3a3a;
    }
    .build-code {
        text-align: left;
        font-size: 18px;
        padding: 8px;
        color: #535252;
    }
</style>
