<template>
    <div id="emoji-list">
        <ul class="emoji-list">
            <li v-for="(item, index) in items" :key="index" :title="index" @click="selectEmoji(item)">{{ item }}</li>
        </ul>
    </div>
</template>

<script>
import emojis from '../assets/emoji-compact.json'

export default {
    name: 'EmojiList',
    props: {},
    data () {
        return {
            items: emojis
        }
    },
    methods: {
        selectEmoji: function (emoji) {
            this.$emit('select-emoji', emoji)
        }
    }
}
</script>

<style scoped>
ul          { list-style: inside none; margin: 0; padding: 14px; display: flex; flex-flow: row wrap; }
li          { flex: 1 0 15%; padding: 6px 0px; font-size: 1.6em; text-align: center; border-radius: 3px; cursor: pointer; }
li:hover    { background-color: #00000022; }

/* RESPONSIVE VIEWS */
@media (min-width: 800px) {
    li { padding: 6px 12px; flex: 1 0; }
}
</style>
