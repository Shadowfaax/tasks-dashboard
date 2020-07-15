<template>
    <div id="app" v-bind:class="[ options.darkMode ? 'dark' : '' ]">
        <div class="app-blk">
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
            <WidgetTime />
            <Dashboard v-bind:add-emoji="selectedEmoji" @openEmojiPanel="toggleEmojiPanel" />
            <div class="dark-mod-btn">
                <div class="switch-btn" @click="switchVMode" v-bind:class="[ options.darkMode ? 'on' : '' ]"><div class="switch"></div></div>
            </div>
            <footer>Dev by <span class="highlight">anthony.prospero@gmail.com</span> <br> <span class="highlight">{{ version }}</span> - powered by Vue.JS</footer>
        </div>
    </div>
</template>

<script>
import Header from './components/Header.vue'
import WidgetTime from './components/WidgetTime.vue'
import Dashboard from './components/Dashboard.vue'
import EmojiList from './components/EmojiList.vue'

export default {
    name: 'App',
    components: {
        Header,
        WidgetTime,
        Dashboard,
        EmojiList
    },
    data: function () {
        return {
            version: 'v0.2.0a',
            options: { darkMode: false },
            news: '',
            popupShow: false,
            selectedEmoji: ''
        }
    },
    methods: {
        loadOptions: function () {
            const storage = localStorage
            const options = storage.getItem('options')
            if (options) this.options = JSON.parse(options)
            else this.saveOptions()
        },
        saveOptions: function () {
            const storage = localStorage
            storage.setItem('options', JSON.stringify(this.options))
        },
        togglePopup: function () {
            this.popupShow = !this.popupShow
        },
        switchVMode: function () {
            this.$set(this.options, 'darkMode', !this.options.darkMode)
            this.saveOptions()
        },
        toggleEmojiPanel: function () {
            this.togglePopup()
        },
        selectEmoji: function (emoji) {
            this.selectedEmoji = emoji
            // console.log('coucou ' + emoji)
            this.toggleEmojiPanel()
        },
        resetEmoji: function (emoji) {
            alert('ù:*;:ù')
        }
    },
    created: function () {
        this.loadOptions()
    }
}
</script>

<style>
/* ANIMATIONS */
@keyframes slide-in-from-left   { from { transform: translateX(-40vw); }  to { transform: translateX(0); } }
@keyframes slide-in-from-right  { from { transform: translateX(40vw); }  to { transform: translateX(0); } }

/* HTML */
* { box-sizing: border-box; transition: all ease .2s, transform ease-in-out .2s, width ease-in-out .2s, background-color ease .2s, box-shadow ease .2s; }
html, body { margin: 0; padding: 0; }
body { background-color: #F0F2F5; }
input { padding: 7px 12px; border: none; border-radius: 3px; background-color: #FFFFFF; font-size: .9em; letter-spacing: 1px; font-family: Helvetica, sans-serif; overflow: hidden; }
input::placeholder { color: #999999AA; }
input[type='submit'],
button { padding: 2px 15px; font-family: 'Roboto', Arial, Helvetica, sans-serif; font-size: 1em; color: #FFFFFFFF; background-color: #3399CCFF; border: none; border-radius: 3px; box-shadow: -1px -1px 0px 1px #00000033 inset; }
input[type='submit']:hover,
button:hover { box-shadow: 1px 1px 0px 1px #00000033 inset; transform: translate(2px, 2px); cursor: pointer; }
form    { width: 100%; }
a       { text-decoration: none; color: #3399CCFF; }
footer  { bottom: 0; left: 0; width: 90%; margin: 30px auto 0 auto; padding: 14px; text-align: center; font-family: Arial, Helvetica, sans-serif; font-size: .8em; line-height: 1.6em; color: #00000044; background-color: #F0F2F5; border-radius: 6px; }

/* GENERIC CLASSES */
.info                   { background-color: #66AACC55; }
.ok                     { background-color: #66AA7755; }
.warning                { background-color: #BBAA4455; }
.error                  { background-color: #DD333355; }
.bg-white               { background-color: #FFFFFFFF; }
.hidden                 { display: none; }
.shrink                 { width: 0; padding: 0; margin: 0; overflow: hidden; }
.h-space-5              { display: inline-block; width:  5px; }
.h-space-10             { display: inline-block; width: 10px; }
.v-space-10             { display: inline-block; height: 10px; }
.v-space-20             { display: inline-block; height: 20px; }
.v-space-30             { display: inline-block; height: 30px; }
.center                 { text-align: center; }
.vertical-space-30      { display: block; width: 100%; height: 30px; }
.vertical-space-50      { display: block; width: 100%; height: 50px; }
.flex-center-items      { display: flex; flex-flow: row nowrap; justify-content: space-between; align-items: center; }
.highlight              { color: #000000AA; }

.switch-btn             { width: 48px; height: 24px; border-radius: 12px; display: flex; flex-flow: row nowrap; align-items: center; background-color: #99CCEEFF; box-shadow: 1px 1px 0 1px #00000044 inset; cursor: pointer; }
.switch-btn .switch     { width: 22px; height: 22px; border-radius: 11px; background-color: #00000066; box-shadow: -1px -1px 0 1px #00000022 inset; }
.switch-btn.on          { background-color: #336688FF; flex-flow: row-reverse nowrap; }
.switch-btn::after      { content: '🌞'; position: relative; top: -1px; left: 1px; font-size: 1.0em; opacity: .5; }
.switch-btn.on::after   { content: '🌚'; left: -2px; }

/* APP */
#app                { width: 100%; min-height: 100vw; padding: 0 2vw; margin: 0 auto; font-family: Avenir, Helvetica, Arial, sans-serif; -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; color: #2C3E50; }
.popup-header       { position: fixed; z-index: 9999; top: 0; left: 0; padding: 10px 0; width: 100%; display: flex; flex-flow: row wrap; justify-content: space-around; align-items: center; background-color: #FFFFFFFF; box-shadow: 0 0 6px #00000022; }
.fullscreen-popup   { position: fixed; z-index: 9990; top: 0; left: 0; width: 100%; height: 100%; background-color: #FFFFFF22; backdrop-filter: blur(16px); overflow: auto; }
.popup-content      { margin-top: 60px; }
.widgets-block      { width: 100%; }
.widget             { flex: 1 3 100%; margin: 8px 0; border-radius: 3px; }

.dark-mod-btn       { margin-top: 20px; display: flex; justify-content: space-around; }

.info-panel-blk     { display: flex; flex-flow: row wrap; justify-content: flex-end; margin: 0; text-align: right; }
.info-panel         { width: 100%; padding: 7px 7px; display: flex; flex-flow: row-reverse wrap; justify-content: space-around; align-items: center; /* flex: 0; */ font-size: 1.0em; color: #FFFFFFFF; border-radius: 6px; cursor: default; }
.info-panel.right   { justify-content: end; }
.info-panel.nobg    { background-color: transparent; border: none; padding: 0; }
.infop-item         { display: inline-block; flex: 0 1 auto; overflow: hidden; white-space: nowrap; height: 30px; margin: 2px 3px; padding: 4px 6px; font-size: 12px; text-transform: uppercase; background-color: #11223322; border-radius: 3px; letter-spacing: 1px; text-align: center; display: flex; align-items: center; }
.infop-item.hidden  { display: none; }
.infop-item-icon    { font-size: 22px; font-weight: 900; text-transform: lowercase; font-family: 'Times New Roman', Times, serif; }
.info-toggle        { display: flex; justify-content: space-around; align-items: center; flex: 0 0 36px; height: 30px; text-align: center; cursor: pointer; color: #2288AAFF; background-color: #FFFFFF00; }
.info-toggle:hover  { background-color: #FFFFFFCC; }

/* DARK MODE */
#app.dark                       { background-color: #303234FF; }
#app.dark footer                { background-color: transparent; color: #FFFFFF33; }
#app.dark .gauge-blk            { background-color: #FFFFFF11; border-color: #444; box-shadow: 1px 1px 0 1px #00000055; }
#app.dark .gauge-items          { background-color: #FFFFFF00; }
#app.dark .gauge-text           { background-color: #222222FF; color: #FFFFFFAA; }
#app.dark .gauge-img .emoji     { background-color: #444; box-shadow: 0px 2px 0 0px #00000055; }
#app.dark input[type="text"],
#app.dark input[type="number"]  { background-color: #FFFFFF11; color: #FFFFFF99; }
#app.dark .title1               { color: #FFFFFFFF; }
#app.dark .highlight            { color: #FFFFFFAA; }

/* RESPONSIVE VIEWS */
@media (min-width: 800px) {
    html, body { height: 100%; margin: 0; padding: 0; }
    #app { min-height: 100vh; }
    .header-block { padding: 36px 0; height: auto; }
    .page-title { font-size: 108px; }
    .app-blk { width: 800px; min-height: 100vh; margin: 0 auto; padding: 50px; padding-top: 10px; background-color: #00000005; }
    .popup-header { width: 800px; left: 50%; margin-left: -400px; }
    .fullscreen-popup { width: 800px; left: 50%; margin-left: -400px; }
    #app.dark .app-blk { background-color: #00000011; }
    #app.dark footer { background-color: #00000011; color: #FFFFFF33; }
}
</style>
