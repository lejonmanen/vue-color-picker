<template>
    <div id="app">
        <h1>App colors</h1>
        <ColorList />
    </div>
</template>

<script>
import ColorList from './components/ColorList.vue'

export default {
    name: 'App',
    components: {
        ColorList
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    /* text-align: center; */
    color: #2c3e50;
    /* margin-top: 60px; */
}
</style>
