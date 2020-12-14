<template>
    <div>
        <div class="loading">
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
            <div class="dot"></div>
        </div>
        <nuxt-link to="/">back</nuxt-link>
    </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({})
</script>

<style lang="stylus" scoped>
.loading
    $colors = #7ef9ff, #89cff0, #4682b4, #0f52ba, #000080
    $idx = 1 .. 5
    display flex

    .dot
        position relative
        width 2em
        height 2em
        margin .8em
        border-radius 50%

        &::before // 相当于.dot::before
            position absolute
            content ''
            width 100%
            height 100%
            background inherit
            border-radius inherit
            animation wave 2s ease-out infinite

        for i in $idx
            &:nth-child({i}) // 相当于.dot:nth-child()
                background $colors[i - 1]

                &::before // 相当于.dot:nth-child()::before
                    animation-delay i * .2s

@keyframes wave
    50%, 75%
        transform scale(2.5)

    80%, 100%
        opacity 0
</style>

