<script lang="ts">

import Icon from "@iconify/svelte";
import { getBgBlur, getDefaultHue, getHue, setBgBlur, setHue } from "@utils/setting-utils";

let hue = getHue();
let bgBlur = getBgBlur();
const defaultHue = getDefaultHue();
let isReady = false;

function resetHue() {
	hue = getDefaultHue();
}

$: if (isReady && (hue || hue === 0)) {
	setHue(hue);
}

$: if (isReady) {
	setBgBlur(bgBlur);
}

import { onMount } from "svelte";

onMount(() => {
	hue = getHue();
	bgBlur = getBgBlur();
	isReady = true;
});
</script>

<div id="display-setting" class="float-panel float-panel-closed absolute transition-all w-80 right-4 px-4 py-4">
    <div class="flex flex-row gap-2 mb-3 items-center justify-between">
        <div class="flex gap-2 font-bold text-lg text-neutral-100 transition relative ml-3
            before:w-1 before:h-4 before:rounded-md before:bg-[var(--primary)]
            before:absolute before:-left-3 before:top-[0.33rem]"
        >
            主题色彩
            <button aria-label="Reset to Default" class="btn-regular w-7 h-7 rounded-md  active:scale-90"
                    class:opacity-0={hue === defaultHue} class:pointer-events-none={hue === defaultHue} on:click={resetHue}>
                <div class="text-[var(--btn-content)]">
                    <Icon icon="fa6-solid:arrow-rotate-left" class="text-[0.875rem]"></Icon>
                </div>
            </button>
        </div>
        <div class="flex gap-1">
            <input aria-label="Hue Value" id="hueValue" type="number" min="0" max="360" bind:value={hue}
                   class="transition bg-[var(--btn-regular-bg)] w-12 h-7 rounded-md text-center font-bold text-sm text-[var(--btn-content)] outline-none"
            />
        </div>
    </div>
    <div class="w-full h-6 px-1 bg-[oklch(0.70_0.10_0)] rounded select-none mb-3">
        <input aria-label="主题色彩" type="range" min="0" max="360" bind:value={hue}
               class="slider" id="colorSlider" step="1" style="width: 100%">
    </div>

    <div class="flex flex-row gap-2 mb-3 mt-3 items-center justify-between">
        <div class="flex gap-2 font-bold text-lg text-neutral-100 transition relative ml-3
            before:w-1 before:h-4 before:rounded-md before:bg-[var(--primary)]
            before:absolute before:-left-3 before:top-[0.33rem]"
        >
            背景模糊
        </div>
        <div class="flex gap-1">
            <div class="transition bg-[var(--btn-regular-bg)] w-10 h-7 rounded-md flex justify-center
            font-bold text-sm items-center text-[var(--btn-content)]">
                {bgBlur}
            </div>
        </div>
    </div>
    <div class="w-full h-6 bg-[var(--btn-regular-bg)] rounded select-none overflow-hidden">
        <input aria-label="背景模糊" type="range" min="0" max="20" bind:value={bgBlur}
               class="slider" step="1" style="width: 100%; --value-percent: {bgBlur / 20 * 100}%">
    </div>
</div>


<style lang="stylus">
    #display-setting
      input[type="number"]
        -moz-appearance textfield
        &::-webkit-inner-spin-button
        &::-webkit-outer-spin-button
          -webkit-appearance none
          margin 0

      input[type="range"]
        -webkit-appearance none
        height 1.5rem
        background-color transparent
        transition background-image 0.15s ease-in-out

        &:not(#colorSlider)
            background-image linear-gradient(to right, var(--primary) 0%, var(--primary) var(--value-percent), transparent var(--value-percent), transparent 100%)

      #colorSlider
        background-image var(--color-selection-bar)

      input[type="range"]
        /* Input Thumb */
        &::-webkit-slider-thumb
          -webkit-appearance none
          height 0
          width 0
          background transparent
          box-shadow none
          border none

        &::-moz-range-thumb
          -webkit-appearance none
          height 0
          width 0
          background transparent
          box-shadow none
          border none

        &::-ms-thumb
          -webkit-appearance none
          height 0
          width 0
          background transparent
          box-shadow none
          border none

      #colorSlider
        background-image var(--color-selection-bar)
        &::-webkit-slider-thumb
          -webkit-appearance none
          height 1rem
          width 0.5rem
          border-radius 0.125rem
          background rgba(255, 255, 255, 0.7)
          box-shadow none
          margin-top 0
          transform none
          transition background 0.15s
          &:hover
            background rgba(255, 255, 255, 0.8)
          &:active
            background rgba(255, 255, 255, 0.6)

        &::-moz-range-thumb
          -webkit-appearance none
          height 1rem
          width 0.5rem
          border-radius 0.125rem
          border-width 0
          background rgba(255, 255, 255, 0.7)
          box-shadow none
          transform none
          transition background 0.15s
          &:hover
            background rgba(255, 255, 255, 0.8)
          &:active
            background rgba(255, 255, 255, 0.6)

        &::-ms-thumb
          -webkit-appearance none
          height 1rem
          width 0.5rem
          border-radius 0.125rem
          background rgba(255, 255, 255, 0.7)
          box-shadow none
          transform none
          transition background 0.15s
          &:hover
            background rgba(255, 255, 255, 0.8)
          &:active
            background rgba(255, 255, 255, 0.6)
</style>
