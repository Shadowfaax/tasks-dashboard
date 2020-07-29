<template>
    <div id="app" v-bind:class="[ options.darkMode ? 'dark' : '' ]">
        <div class="app-blk-ovf">
            <div class="menu"></div>
            <div class="burger-menu" :class="[menuShow ? 'active' : '']" @click="toggleMenu">
                <div class="burger-menu-stripe"></div><div class="burger-menu-stripe"></div><div class="burger-menu-stripe"></div>
            </div>
            <div class="app-blk" :class="[menuShow ? 'min' : '']">
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
                <div class="widget tips" v-show="options.showTips">
                    <div class="tips-text">{{ tips }}</div>
                    <div class="clearer"></div>
                    <div class="tips-close" @click="switchTipsShow">Don't show tips again</div>
                    <div class="clearer"></div>
                </div>
                <WidgetTime />
                <Dashboard v-bind:add-emoji="selectedEmoji" @openEmojiPanel="toggleEmojiPanel" />
                <div class="dark-mod-btn">
                    <div class="switch-btn" @click="switchVMode" v-bind:class="[ options.darkMode ? 'on' : '' ]"><div class="switch"></div></div>
                </div>
                <footer>Dev by <a href="mailto:anthony.prospero@gmail.com" class="highlight">anthony.prospero@gmail.com</a> <br> <span class="highlight">{{ version }}</span> - powered by Vue.JS</footer>
                <div class="v-space-20"></div>
            </div>
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
            options: { darkMode: false, showTips: true },
            news: '',
            tips: 'TraqOn is a web-app which allows you to keep track on different activities in a stressless way. For example if you need to remind yourself to water your cactus every 2 weeks, just set a track on 14 days. Then when you just did your task, click on the reset button to restart the tracker',
            popupShow: false,
            menuShow: false,
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
        switchVMode: function () {
            this.$set(this.options, 'darkMode', !this.options.darkMode)
            this.saveOptions()
        },
        switchTipsShow: function () {
            this.$set(this.options, 'showTips', !this.options.showTips)
            this.saveOptions()
        },
        toggleMenu: function () {
            this.menuShow = !this.menuShow
        },
        togglePopup: function () {
            this.popupShow = !this.popupShow
        },
        toggleEmojiPanel: function () {
            this.togglePopup()
        },
        selectEmoji: function (emoji) {
            this.selectedEmoji = emoji
            this.toggleEmojiPanel()
        },
        resetEmoji: function (emoji) {
            alert('resetEmoji')
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
a:hover { text-decoration: underline; }
footer  { bottom: 0; left: 0; width: 90%; margin: 30px auto 0 auto; padding: 14px; text-align: center; font-family: Arial, Helvetica, sans-serif; font-size: .8em; line-height: 1.6em; color: #00000044; background-color: #F0F2F577; border-radius: 6px; }

/* GENERIC CLASSES */
.info                   { background-color: #66AACC55; }
.ok                     { background-color: #66AA7755; }
.warning                { background-color: #BBAA4455; }
.error                  { background-color: #DD333355; }
.bg-white               { background-color: #FFFFFFFF; }
.bg-dark-yellow         { background-color: #CCAA44FF; }
.hidden                 { display: none; }
.clearer                { clear: both; }
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
#app                { width: 100%; min-height: 100vh; margin: 0 auto; font-family: Avenir, Helvetica, Arial, sans-serif; -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; color: #2C3E50; background-color: #E0E0E0; }
.app-blk-ovf        { width: 100%; min-height: 100vh; overflow-x: hidden; background-color: #FFFFFF11; }
.app-blk            { height: 100vh; overflow: auto; position: relative; z-index: 600; padding: 0 2vw; background-color: #D4D6D8FF; }
.app-blk.min        { height: 100vh; overflow: hidden; transform: translateX(80vw) scale(0.8); border-radius: 6px; }

.menu               { width: 100%; min-height: 100vh; overflow-x: hidden; overflow-y: auto; position: fixed; z-index: 400; background-color: #66AACCFF; }
.burger-menu        { position: absolute; z-index: 888; left: 6px; top: 6px; width: 30px; height: 23px; display: flex; flex-flow: column nowrap; justify-content: space-between; align-items: center; overflow: hidden; cursor: pointer; }
.burger-menu-stripe { width: 100%; height: 3px; background-color: #444444FF; border-radius: 0; }
.burger-menu.active .burger-menu-stripe:nth-child(1) { position: relative; top: -1.5px; transform: rotateZ(37deg) scaleX(1.5); transform-origin: center left; border-radius: 0; }
.burger-menu.active .burger-menu-stripe:nth-child(2) { opacity: 0; }
.burger-menu.active .burger-menu-stripe:nth-child(3) { position: relative; top: 1.5px; transform: rotateZ(-38deg) scaleX(1.5); transform-origin: center left; border-radius: 0; }

.popup-header       { position: fixed; z-index: 9999; top: 0; left: 0; padding: 10px 0; width: 100%; display: flex; flex-flow: row wrap; justify-content: space-around; align-items: center; background-color: #FFFFFFFF; box-shadow: 0 0 6px #00000022; }
.fullscreen-popup   { position: fixed; z-index: 9990; top: 0; left: 0; width: 100%; height: 100%; background-color: #FFFFFF22; backdrop-filter: blur(16px); overflow: auto; }
.popup-content      { margin-top: 60px; }
.widgets-block      { width: 100%; }
.widget             { flex: 1 0 100%; margin: 8px 0; border-radius: 3px; }

.widget.tips        { margin-bottom: 20px; padding: 20px; border-radius: 6px; border: 2px dashed #FFFFFFAA; background-color: #EEDD88FF; }
.tips-text          { font-size: 15px; letter-spacing: 1px; font-family: 'Roboto', Helvetica, Arial, sans-serif; text-align: justify; line-height: 22px; color: #00000088; }
.tips-close         { float: right; margin-top: 10px; padding: 3px 6px; border-radius: 3px; font-size: 14px; letter-spacing: 1px; cursor: pointer; color: #FFFFFFFF; background-color: #00000022; }
.tips-close:hover   { background-color: #00000044; }

.dark-mod-btn       { margin-top: 20px; display: flex; justify-content: space-around; }

.info-panel-blk     { display: flex; flex-flow: row wrap; justify-content: flex-end; margin: 0; text-align: right; }
.info-panel         { width: 100%; padding: 7px 7px; display: flex; flex-flow: column wrap; justify-content: space-around; align-items: flex-end; /* flex: 0; */ font-size: 1.0em; color: #FFFFFFFF; border-radius: 6px; cursor: default; }
.info-panel.right   { justify-content: end; }
.info-panel.nobg    { background-color: transparent; border: none; padding: 0; }
.infop-item         { display: inline-block; flex: 0 1 auto; overflow: hidden; white-space: nowrap; height: 30px; margin: 2px 3px; padding: 4px 6px; font-size: 12px; text-transform: uppercase; background-color: #11223322; border-radius: 3px; letter-spacing: 1px; text-align: center; display: flex; align-items: center; }
.infop-item.hidden  { display: none; }
.infop-item-icon    { font-size: 18px; font-weight: 900; text-transform: lowercase; font-family: 'Times New Roman', Times, serif; }
.info-toggle        { display: flex; justify-content: space-around; align-items: center; flex: 0 0 36px; height: 30px; text-align: center; cursor: pointer; color: #2288AAFF; background-color: #FFFFFF00; }
.info-toggle:hover  { background-color: #FFFFFFCC; }

/* DARK MODE */
#app.dark                       { background-color: #303234FF; }
#app.dark .app-blk              { background-color: #333537FF; }
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
    .app-blk-ovf { width: 800px; min-height: 100vh; margin: 0 auto; overflow-x: hidden; background-color: #FFFFFF11; }
    .app-blk { width: 800px; min-height: 100vh; margin: 0 auto; padding: 50px; padding-top: 10px; }
    .app-blk.min { transform: translateX(600px) scale(0.8); }
    .menu { width: 800px; min-height: 100vh; margin: 0 auto; padding: 50px; }
    .burger-menu { left: 50%; top: 20px; margin-left: -380px; }
    .popup-header { width: 800px; left: 50%; margin-left: -400px; }
    .fullscreen-popup { width: 800px; left: 50%; margin-left: -400px; }
    /* DARK MODE */
    #app.dark footer { background-color: #00000011; color: #FFFFFF33; }
}
</style>
