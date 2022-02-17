<template>
    <div id="app">
        <page-side></page-side>
        <write-page></write-page>
    </div>
</template>

<script>
import WritePage from "@/components/WritePage";
import PageSide from "@/components/PageSide";

export default {
    name: 'App',
    components: {
        PageSide,
        WritePage
    }
}
</script>

<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    display: flex;
}
</style>
