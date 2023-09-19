<script setup>
import { provide, ref, watch } from "vue";


const props = defineProps({
    overlayClass: { type: String, },
    alignment: { type: String, default: 'left' },
    open: { type: Boolean, default: false },
})
const openInner = ref(props.open)
const show = () => {
    window.dispatchEvent(new Event('click'))
    openInner.value = true
}
const hide = () => openInner.value = false
const emit = defineEmits([ 'update:open' ])
watch(() => props.open, value => openInner.value = value)
watch(() => openInner.value, value => {
    if (props.open !== value) emit("update:open", value);
    const handler = value ? window.addEventListener : window.removeEventListener
    handler('click', hide)
})

provide('hide', hide)

</script>

<template>
    <div class="dropdown">
        <div class="dropdown-trigger" @click.stop.capture="show">
            <slot/>
        </div>
        <div :class="{open: openInner, [`align-${alignment}`]: true, [overlayClass]: !!overlayClass}"
             class="dropdown-overlay"
             @click.stop>
            <slot name="overlay" v-bind="{open:openInner}"/>
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
        opacity: 0;
        z-index: -1;
        border-radius: 6px;
        border: solid 1px #d9d9d9;
        background-color: #fff;
        box-shadow: rgba(0, 0, 0, 0.02) 0 4px 8px;
        flex-direction: column;
        transition: all 0.1s linear;
        pointer-events: none;

        &.align-left {
            left: 0;
        }

        &.align-right {
            right: 0;
        }

        &.open {
            display: flex;
            opacity: 1;
            z-index: 9999;
            pointer-events: all;
        }
    }
}
</style>