<template>
    <div class="header-block">
        <div class="page-title-block">
            <div class="page-title"><span class="title1">{{ pageTitle1 }}</span><span class="title2">{{ pageTitle2 }}</span></div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Header',
    props: {
        msg: String,
        pageTitle1: String,
        pageTitle2: String
    }
}
</script>

<style>
.header-block   { padding: 10px 0 0 0; height: 110px; display: flex; justify-content: space-around; align-items: center; }
.page-title     { text-align: center; font-size: 60px; font-weight: 700; font-family: 'Passion One'; letter-spacing: 1px; cursor: default; }
.title1         { color: #50565A; text-shadow: 3px 0px 0px #50565A66; }
.title2         { color: #EE6666; text-shadow: 3px 0px 0px #EE666666; margin-left: 3px; }
</style>
