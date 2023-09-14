<script setup>
import { ref, provide } from "vue";

const open = ref(false)
const show = () => open.value = true
const hide = () => open.value = false

const props = defineProps({
    overlayClass: {
        type: String,
    }
})

provide('hide', hide)
</script>

<template>
    <div class="dropdown">
        <div class="dropdown-trigger" @click.stop.prevent="show">
            <slot/>
        </div>
        <div :class="{open, [overlayClass]: !!overlayClass}" class="dropdown-overlay" @click.stop>
            <slot name="overlay" v-bind="{open}"/>
        </div>
    </div>
</template>

<style scoped lang="less">
.dropdown {
    position: relative;
    overflow: visible;

    &-trigger {
        display: inline-flex;
    }

    &-overlay {
        position: absolute;
        top: 100%;
        margin-top: 4px;
        left: 0;
        opacity: 0;
        z-index: -1;
        border-radius: 6px;
        border: solid 1px #d9d9d9;
        background-color: #fff;
        box-shadow: rgba(0, 0, 0, 0.02) 0 4px 8px;
        flex-direction: column;
        transition: all 0.1s linear;

        &.open {
            display: flex;
            opacity: 1;
            z-index: 9999;
        }
    }
}
</style>