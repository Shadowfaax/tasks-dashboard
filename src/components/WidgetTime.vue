<template>
    <div id="widget-time">
        <div class="widget flex-center-items widget-time-blk">
            <div class="widget-date"><span class="time-emoji">📅</span>{{ date }}</div>
            <div class="widget-time"><span class="time-emoji">🕓</span>{{ time }}</div>
            <div class="widget-chrono hidden"></div>
        </div>
    </div>
</template>

<script>
export default {
    props: {},
    data () {
        return {
            time: '',
            date: ''
        }
    },
    methods: {
        setTime: function () {
            // 🗓️📆📅⏱️⏰🕰️🕓
            var d = new Date()
            var sec = d.getSeconds()
            var min = d.getMinutes()
            var hour = d.getHours()
            var colon = (sec % 2 === 0) ? ':' : ' '
            var emoji = ''

            if (min < 10) min = '0' + min
            if (hour < 10) hour = '0' + hour
            if (sec < 10) sec = '0' + sec
            colon = ':'

            this.time = emoji + hour + colon + min + colon + sec

            const fct = this.setTime
            setTimeout(function () { fct() }, 1000)
        },
        setDate: function () {
            var d = new Date()
            var day = d.getDate()
            var month = d.getMonth()
            var year = d.getFullYear()
            var emoji = ''

            if (day < 10) day = '0' + day
            if (month < 10) month = '0' + (month + 1)
            var sep = '.'

            this.date = emoji + day + sep + month + sep + year

            const fct = this.setDate
            setTimeout(function () { fct() }, 1000)
        }
    },
    created: function () {
        this.setTime()
        this.setDate()
    }
}
</script>

<style scoped>
.widget-time-blk { justify-content: end; }
.widget-date, .widget-time { flex: 0 1 auto; margin: 0 10px 0 0; font-family: 'Roboto', Arial, Helvetica, sans-serif; font-weight: 300; font-size: 1.4em; letter-spacing: 2px; text-align: center; color: #00000099; background-color: #00224411; cursor: default; }
.widget-date:hover, .widget-time:hover { transform: scale(1.1); }
.time-emoji { display: none; }
/* DARK MODE */
#app.dark .widget-date, #app.dark .widget-time { background-color: #FFFFFF11; color: #FFFFFF66; }

/* RESPONSIVE VIEWS */
@media (min-width: 800px) {
    .widget-date, .widget-time { padding: 4px 12px; border-radius: 3px; font-size: 1.8em; }
    .time-emoji { display: inline; position: relative; left: -4px; top: -3px; font-size: .7em; color: #FFF; filter: opacity(.4); }
}
</style>
