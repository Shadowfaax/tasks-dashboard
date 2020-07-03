<template>
    <div id="app">
        <div></div>
        <div class="hidden">{{ news }}</div>
        <Header page-title1="Traq" page-title2="Øn" />
        <Dashboard />
    </div>
</template>

<script>
import Header from './components/Header.vue'
import Dashboard from './components/Dashboard.vue'

export default {
    name: 'App',
    components: {
        Header,
        Dashboard
    },
    data: function () {
        return {
            news: ''
        }
    }
}
</script>

<style>
/* APP */
* { box-sizing: border-box; }
html, body { margin: 0; padding: 0; }
body { background-color: #FFFFFF; }
input { padding: 7px 12px; border: none; border-radius: 3px; background-color: #FFFFFF; font-size: .9em; letter-spacing: 1px; font-family: Helvetica, sans-serif; overflow: hidden; }
input::placeholder { color: #999999AA; }

#app { width: 90vw; margin: 0 auto; font-family: Avenir, Helvetica, Arial, sans-serif; -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; color: #2C3E50; }

/* GENERIC COMPONENTS */
.hidden             { display: none; }
.center             { text-align: center; }
.vertical-space-30  { display: block; width: 100%; height: 30px; }
.vertical-space-50  { display: block; width: 100%; height: 50px; }
.flex-center-items  { display: flex; flex-flow: row nowrap; justify-content: space-between; align-items: center; }

/* RESPONSIVE VIEWS */
@media (min-width: 800px) {

    html, body { height: 100%; margin: 0; padding: 0; }

    #app { width: 800px; min-height: 100%; margin: 0 auto; padding: 50px; background-color: #00000005; }
    input { width: 200px; }

    /* .widget { padding: 10px; border: 1px solid #0000000F; } */
}
</style>
