<template>
    <div id="app">
        <div v-if="popupShow" class="popup-header">
            <button id="popup-close" @click.prevent="togglePopup">Close</button>
        </div>
        <div v-if="popupShow" id="popup" class="fullscreen-popup">
            <div class="popup-content">
                <EmojiList @select-emoji="selectEmoji($event)" />
            </div>
        </div>
        <div class="hidden">{{ news }}</div>
        <Header page-title1="Traq" page-title2="Øn" />
        <Dashboard v-bind:add-emoji="selectedEmoji" @openEmojiPanel="toggleEmojiPanel" />
        <footer>Dev by anthony.prospero@gmail.com <br> {{ version }} - powered by Vue.JS</footer>
    </div>
</template>

<script>
import Header from './components/Header.vue'
import Dashboard from './components/Dashboard.vue'
import EmojiList from './components/EmojiList.vue'

export default {
    name: 'App',
    components: {
        Header,
        Dashboard,
        EmojiList
    },
    data: function () {
        return {
            version: 'v0.1.1a',
            news: '',
            popupShow: false,
            selectedEmoji: ''
        }
    },
    methods: {
        togglePopup: function () {
            this.popupShow = !this.popupShow
        },
        toggleEmojiPanel: function () {
            this.togglePopup()
        },
        selectEmoji: function (emoji) {
            this.selectedEmoji = emoji
            this.toggleEmojiPanel()
        }
    }
}
</script>

<style>
/* APP */
* { box-sizing: border-box; transition: transform ease-in-out .2s, width ease-in-out .2s, background-color ease .2s; }
html, body { margin: 0; padding: 0; }
body { background-color: #F0F2F5; }
input { padding: 7px 12px; border: none; border-radius: 3px; background-color: #FFFFFF; font-size: .9em; letter-spacing: 1px; font-family: Helvetica, sans-serif; overflow: hidden; }
input::placeholder { color: #999999AA; }
input[type='submit'],
button { padding: 2px 15px; font-family: 'Roboto', Arial, Helvetica, sans-serif; font-size: 1em; color: #FFFFFFFF; background-color: #3399CCFF; border: none; border-radius: 3px; box-shadow: -1px -1px 0px 1px #00000033 inset; }
input[type='submit']:hover,
button:hover { box-shadow: 1px 1px 0px 1px #00000033 inset; transform: translate(2px, 2px); }
form { width: 100%; }
a { text-decoration: none; color: #3399CCFF; }
footer { position: fixed; z-index: 555; bottom: 0; left: 0; width: 100%; padding: 10px 0; text-align: center; font-family: Arial, Helvetica, sans-serif; font-size: .8em; color: #00000044; background-color: #F0F2F5; }

#app { width: 90vw; margin: 0 auto; padding-bottom: 50px; font-family: Avenir, Helvetica, Arial, sans-serif; -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; color: #2C3E50; }
.popup-header { position: fixed; z-index: 9999; top: 0; left: 0; padding: 10px 0; width: 100%; display: flex; flex-flow: row wrap; justify-content: space-around; align-items: center; background-color: #FFFFFFFF; box-shadow: 0 0 6px #00000022; }
.fullscreen-popup { position: fixed; z-index: 9990; top: 0; left: 0; width: 100%; height: 100%; background-color: #FFFFFF22; backdrop-filter: blur(16px); overflow: auto; }
.popup-content { margin-top: 60px; }

/* GENERIC COMPONENTS */
.hidden             { display: none; }
.center             { text-align: center; }
.vertical-space-30  { display: block; width: 100%; height: 30px; }
.vertical-space-50  { display: block; width: 100%; height: 50px; }
.flex-center-items  { display: flex; flex-flow: row nowrap; justify-content: space-between; align-items: center; }

.infop-item         { display: inline-block; margin: 0 3px; padding: 4px 6px; font-size: 11px; text-transform: uppercase; background-color: #33445533; border-radius: 3px; text-align: center; display: flex; align-items: center; }
.infop-item-icon    { font-size: 14px; margin-right: 6px; }
.info-panel         { background-color: #E9E9E9FF; width: 100%; padding: 14px; margin: 24px 0; display: flex; justify-content: space-around; align-items: center; font-size: 1.0em; color: #FFFFFFFF; border: 1px dashed #FFFFFFCC; border-radius: 9px; }
.info-panel.right   { justify-content: flex-end; }
.info-panel.nobg    { background-color: transparent; border: none; padding: 0; }
.info-panel.info    { background-color: #66AACC55; }
.info-panel.ok      { background-color: #66AA7755; }
.info-panel.warning { background-color: #BBAA4455; }
.info-panel.error   { background-color: #DD333355; }

/* RESPONSIVE VIEWS */
@media (min-width: 800px) {

    html, body { height: 100%; margin: 0; padding: 0; }

    #app { width: 800px; min-height: 100%; margin: 0 auto; padding: 50px; padding-top: 10px; background-color: #00000005; }

    .popup-header { width: 800px; left: 50%; margin-left: -400px; }
    .fullscreen-popup { width: 800px; left: 50%; margin-left: -400px; }

    /* .widget { padding: 10px; border: 1px solid #0000000F; } */
}
</style>
