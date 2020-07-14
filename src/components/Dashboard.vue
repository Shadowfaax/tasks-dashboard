<template>
    <div class="widgets-block">

        <div class="info-panel-blk">
            <div class="info-panel info right">
                <span  v-bind:class="[displayInfo ? '' : 'shrink']" class="infop-item"><span class="infop-item-icon">⏱️</span><div class="h-space-5"></div> gauge timer</span>
                <span  v-bind:class="[displayInfo ? '' : 'shrink']" class="infop-item"><span class="infop-item-icon">❌</span><div class="h-space-5"></div>remove gauge</span>
                <span @click="displayInfoToggle()" class="infop-item info-toggle"><span href="#" class="infop-item-icon">ℹ️</span></span>
            </div>
        </div>

        <div id="gauge-widgets" class="gauge-widgets-container">
            <div v-for="(item, index) in items" v-bind:key="item.submitTimestamp" class="widget gauge">
                <div class="gauge-blk" @click="gaugeCtrlToggle(index)">
                    <div class="gauge-items flex-center-items">
                        <div class="gauge-img flex-center-items"><span class="emoji" v-html="item.emoji"></span></div>
                        <div class="gauge-text">{{ item.name }}</div>
                    </div>
                    <div class="gauge-progress" v-bind:class="[item.percent > 66 ? 'yellow' : '', item.percent > 90 ? 'red' : '', item.percent >= 100 ? 'dark-red' : '']" v-bind:style="'width:'+item.percent+'%'"></div>
                </div>
                <a href="#" title="Reset timer" v-bind:class="[gaugeToggle[index] ? 'shrink' : '']" class="gauge-action gauge-reset" :data-array-id="index" @click="resetGauge(index)">
                    <span>⏱️</span>
                </a>
                <a href="#" title="Remove gauge" v-bind:class="[gaugeToggle[index] ? 'shrink' : '']" class="gauge-action gauge-delete" :data-array-id="index" @click="removeNode(index)">
                    <span>❌</span>
                </a>
            </div>
        </div>

        <div class="widget add">
            <button class="button-add" @click="toggleForm">{{ addButton }}</button>
        </div>

        <div v-if="formShow" class="widget add-widgets-block">
            <form v-on:submit.prevent="submitAddForm">
                <div class="add-widgets-inline-blk">
                    <div class="input-blk"><input data-regex="[a-zA-Z0-9]+" id="add-name" v-model="addName" type="text" placeholder="Name"></div>
                </div>
                <div class="add-widgets-inline-blk">
                    <div class="input-blk"><input data-regex="" id="add-emoji" v-model="addEmoji" type="hidden" placeholder="Emoji"><button @click.prevent="openEmojiPanel">Emoji {{ addEmoji }}</button></div>
                    <div class="input-blk"><input type="number" min="0" max="10000" data-regex="[0-9]+" id="add-days" @keydown="checkForm" v-model="addDays" placeholder="Days"></div>
                </div>
                <div class="add-widgets-inline-blk">
                    <div class="input-blk"><input type="number" min="0" max="72" data-regex="[0-9]+" id="add-hours" @keydown="checkForm" v-model="addHours" placeholder="Hours"></div>
                    <div class="input-blk"><input type="number" min="0" max="999" data-regex="[0-9]+" id="add-minutes" @keydown="checkForm" v-model="addMinutes" placeholder="Minutes"></div>
                    <div class="input-blk"><input type="number" min="0" max="999" data-regex="[0-9]+" id="add-seconds" @keydown="checkForm" v-model="addSeconds" placeholder="Seconds"></div>
                </div>
                <div class="add-widgets-inline-blk">
                    <div class="input-blk center"><input id="add-form-submit" type="submit" value="Submit"></div>
                </div>
            </form>
        </div>

    </div>
</template>

<script>
// Form submit
function submitAddForm () {
    const storage = localStorage
    var inputArray = {}

    // TODO: Use Vue binds
    inputArray['add-name'] = document.getElementById('add-name').value ? document.getElementById('add-name').value : '- - - - -'
    inputArray['add-emoji'] = document.getElementById('add-emoji').value ? document.getElementById('add-emoji').value : '⏱️'
    inputArray['add-days'] = document.getElementById('add-days').value ? document.getElementById('add-days').value : 0
    inputArray['add-hours'] = document.getElementById('add-hours').value ? document.getElementById('add-hours').value : 0
    inputArray['add-minutes'] = document.getElementById('add-minutes').value ? document.getElementById('add-minutes').value : 0
    inputArray['add-seconds'] = document.getElementById('add-seconds').value ? document.getElementById('add-seconds').value : 0

    var newData = {
        name: inputArray['add-name'],
        emoji: inputArray['add-emoji'],
        submitTimestamp: Date.now(),
        targetTimestamp: Date.now() + (((inputArray['add-days'] * 24 + inputArray['add-hours']) * 60 + inputArray['add-minutes']) * 60 + inputArray['add-seconds']) * 1000
    }

    // Getting existing data from local storage
    let dataS = storage.getItem('data')
    if (!dataS) {
        dataS = '{ "dataArray": [] }'
    }
    var newDataObj = JSON.parse(dataS)

    // Pushing new data
    newDataObj.dataArray.push(newData)
    storage.setItem('data', JSON.stringify(newDataObj))
}

export default {
    name: 'Dashboard',
    props: {
        addEmoji: String
    },
    data: function () {
        return {
            items: [],
            gaugeToggle: [],
            displayInfo: false,
            addName: '',
            addDays: '',
            addHours: '',
            addMinutes: '',
            addSeconds: '',
            formShow: false,
            addButton: '+'
        }
    },
    methods: {
        checkForm: function (event) {
            // checkForm(event)
        },
        displayInfoToggle: function (event) {
            this.displayInfo = !this.displayInfo
        },
        resetForm: function (event) {
            this.addName = ''
            this.addDays = ''
            this.addHours = ''
            this.addMinutes = ''
            this.addSeconds = ''
        },
        submitAddForm: function (event) {
            submitAddForm()
            this.loadData()
            this.resetForm()
            this.toggleForm()
        },
        loadData: function () {
            const storage = localStorage
            // TODO: Add error handler
            const sampleData = '{"dataArray":[{"name":"Water the cactus","emoji":"🌵","submitTimestamp":1593982453700,"targetTimestamp":1594587253700},{"name":"Boiler maintenance","emoji":"🚿","submitTimestamp":1593982571997,"targetTimestamp":1594587371997},{"name":"Feed the fishes","emoji":"🐠","submitTimestamp":1593982626926,"targetTimestamp":1594155426926},{"name":"Clean litter box","emoji":"🐈","submitTimestamp":1593982693072,"targetTimestamp":1594846693072},{"name":"10 minutes chrono","emoji":"⏱️","submitTimestamp":1593982741161,"targetTimestamp":1680383341161}]}'
            if (!storage.getItem('data')) {
                localStorage.setItem('data', sampleData)
            }
            this.items = JSON.parse(storage.getItem('data')).dataArray
            for (let i = 0; i < this.items.length; i++) {
                var diff = this.items[i].targetTimestamp - this.items[i].submitTimestamp
                this.items[i].now = Date.now()
                var nowDiff = this.items[i].now - this.items[i].submitTimestamp
                var percent = nowDiff / diff * 100
                if (percent > 100) percent = 100
                this.items[i].percent = parseFloat(percent).toFixed(2)
            }
        },
        refreshData: function () {
        },
        openEmojiPanel: function () {
            this.$emit('openEmojiPanel')
        },
        toggleForm: function () {
            this.formShow = !this.formShow
            this.addButton = this.formShow ? '-' : '+'
            if (this.formShow) setTimeout(function () { window.scrollTo(0, document.body.scrollHeight) }, 50)
            else window.scrollTo(0, 0)
        },
        setGaugeToggle: function (ex = -1) {
            for (let i = 0; i < this.items.length; i++) {
                if (i !== ex) this.$set(this.gaugeToggle, i, true)
            }
        },
        gaugeCtrlToggle: function (index) {
            this.$set(this.gaugeToggle, index, !this.gaugeToggle[index])
            this.setGaugeToggle(index)
            // setTimeout(this.$set(this.gaugeToggle, index, true), 3000)
        },
        removeNode: function (index) {
            const storage = localStorage
            this.items.splice(index, 1)
            var data = { dataArray: this.items }
            var dataStr = JSON.stringify(data)
            storage.setItem('data', dataStr)
            this.gaugeCtrlToggle(index)
        },
        resetGauge: function (index) {
            const storage = localStorage
            const diff = this.items[index].targetTimestamp - this.items[index].submitTimestamp
            this.items[index].targetTimestamp = Date.now() + diff
            this.items[index].submitTimestamp = Date.now()
            var data = { dataArray: this.items }
            var dataStr = JSON.stringify(data)
            storage.setItem('data', dataStr)
            this.loadData()
            this.gaugeCtrlToggle(index)
        },
        animGauge: function (timer = 1000) {
            this.loadData()
            const fct = this.animGauge
            setTimeout(function () { fct(timer) }, timer)
        }
    },
    created: function () {
        this.loadData()
        this.animGauge(50)
        this.setGaugeToggle()
    }
}

</script>

<style>
.widget-block           { width: 100%; display: flex; flex-flow: row wrap; }
.widget                 { flex: 1 3 100%; margin: 16px 0; border-radius: 3px; }

.gauge                      { display: flex; flex-flow: row nowrap; justify-content: space-between; align-items: center; }
.gauge-action               { /* flex: 1 0 36px; */ margin: 0 0 0 5px; width: 36px; height: 36px; border-radius: 3px; display: flex; justify-content: space-around; align-items: center; font-size: 1.2em; overflow: hidden; font-family: Arial, Helvetica, sans-serif; font-size: 1.2em; text-transform: uppercase; color: #FFFFFFCC; }
.gauge-action.shrink        { width: 0; padding: 0; margin: 0; }
.gauge-action:hover         { box-shadow: 1px 1px 0px 1px #00000033 inset; transform: translate(1px, 1px); }
.gauge-reset                { background-color: #AACCCCCC; box-shadow: -1px -1px 0 1px #00000022 inset; }
.gauge-delete               { background-color: #BB998866; box-shadow: -1px -1px 0 1px #00000022 inset; }
.gauge-blk                  { flex: 1 1 60%; height: 32px; margin-left: 36px; border-radius: 3px; padding: 0; border: 4px solid #FFFFFF; background-color: #FFFFFFCC; box-shadow: -1px -1px 0 1px #11224422 inset, 2px 2px 20px #00000000; background-image: url('../../public/images/patterns/black-twill.png'); box-sizing: content-box; }
.gauge-blk:hover            { transform: translateX(-1%); cursor: pointer; box-shadow: -1px -1px 0 1px #11224422 inset, 0 4px 14px #00000011; }
.gauge-items                { width: 100%; height: 100%; background-color: #FFFFFFCC; }
.gauge-img                  { position: relative; top: 0px; left: -42px; z-index: 60; }
.gauge-img .emoji           { display: flex; align-items: center; justify-content: center; position: absolute; width: 42px; height: 42px; border-radius: 3px; text-align: center; font-size: 1.4em; background-color: #FFFFFFFF; box-shadow: 0 0 5px #11224422; }
.gauge-text                 { display: inline-block; white-space: nowrap; padding-right: 6px; position: relative; z-index: 60; font-size: .8em; text-transform: uppercase; font-family: 'Roboto'; color: #00000077; background-color: #FFFFFFFF; }
.gauge-progress             { height: 100%; width: 33%; position: relative; top: -32px; z-index: 50; background-color: #00CC99FF; border-top-right-radius: 3px; border-bottom-right-radius: 3px; }
.gauge-progress.yellow      { background-color: #EEBB44FF; }
.gauge-progress.red         { background-color: #DD4444FF; }
.gauge-progress.dark-red    { background-color: #330000BB; }

.widget.add             { display: flex; justify-content: space-around; align-items: center; }
.button-add             { margin: 0 auto; width: 30%; max-width: 100px; font-size: 1.4em; cursor: pointer; background-color: #55CCDD; }

.add-widgets-block      { width: 100%; display: flex; flex-flow: row wrap; justify-content: space-between; }
.add-widgets-inline-blk { width: 100%; display: flex; flex-flow: row wrap; justify-content: space-between; align-items: center; height: 36px; }
.add-widgets-inline-blk .input-blk { flex: 1 1 30%; margin-left: 2%; }
.add-widgets-inline-blk .input-blk:nth-child(1) { margin-left: 0; }
.add-widgets-inline-blk .input-blk input { width: 100%; background-color: #00000011; }
.add-widgets-inline-blk .input-blk input[type='submit'],
.add-widgets-inline-blk .input-blk button { width: 100%; padding: 5px 15px; font-family: 'Roboto', Arial, Helvetica, sans-serif; font-size: 1em; color: #FFFFFFFF; background-color: #3399CCFF; border: none; border-radius: 3px; box-shadow: -1px -1px 0px 1px #00000033 inset; }
.add-widgets-inline-blk .input-blk input[type='submit']:hover,
.add-widgets-inline-blk .input-blk button:hover { box-shadow: 1px 1px 0px 1px #00000033 inset; transform: translate(2px, 2px); }
</style>
