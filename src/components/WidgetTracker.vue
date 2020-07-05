<template>
    <div class="widgets-block">

        <div class="widget tracker hidden">
            <div class="tracker-img-blk">
                <div class="tracker-item tracker-img">&#128540;</div>
            </div>
            <div class="tracker-col-blk">
                <div class="tracker-title-blk">
                    <div class="tracker-item tracker-title">Global</div>
                </div>
                <div class="tracker-jauge-blk">
                    <div class="tracker-item tracker-jauge">======</div>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
export default {
    name: 'WidgetTracker',
    props: {},
    data: function () {
        return {
            items: [
                { emoji: '&#128032;', name: 'Feed the fish', percent: 60 },
                { emoji: '&#128570;', name: 'Clean the litter', percent: 33 },
                { emoji: '&#127793;', name: 'Water Geranium', percent: 92 },
                { emoji: '&#128705;', name: 'boiler maintenance', percent: 12 }
            ]
        }
    }
}
</script>

<style>

.tracker                { display: flex; flex-flow: row wrap; justify-content: space-between; align-items: center; overflow: hidden; border-radius: 3px; background-color: #FFFFFFCC; box-shadow: 0 7px 16px #00000011; }
.tracker-img-blk        { display: flex; flex-flow: column nowrap; justify-content: space-between; align-items: center; }
.tracker-col-blk        { display: flex; flex-flow: column nowrap; justify-content: space-between; align-items: center; }
.tracker-item           { display: flex; flex-flow: row wrap; justify-content: space-around; align-items: center; }
.tracker-img            { width: 20vw; height: 20vw; display: flex; justify-content: space-around; align-items: center; }
.tracker-title          { width: 64vw; height: 9.0vw; font-family: 'Roboto'; letter-spacing: 1px; font-size: .9em; }
.tracker-jauge          { width: 64vw; height: 9.0vw; }

</style>
