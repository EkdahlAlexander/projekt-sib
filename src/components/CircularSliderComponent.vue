<template>
<round-slider
    ref="slider1"
    v-bind:change="chgHandler"
    radius="120"
    v-on:input="pInput"
    :create="onSliderCreate"
    min="0"
    width="30"
    max="360"
    step="10"
    start-angle="90"
    end-angle="450" ></round-slider>
</template>

<script setup>
import {defineEmits, ref} from 'vue'
import { RoundSlider } from "vue-round-slider/src/index2.js"

const emits = defineEmits(["sliderValue"]);
const slider1 = ref();
const onSliderCreate = () => {
    if(slider1.value) {
    const obj = slider1?.value.instance;
    var o = obj.options;
    var min = o.min,
    max = o.max;

    // logic for the number generation around the circle
    for (var i = min; i < max; i += 10) {
    var angle = obj._valueToAngle(i);
    var numberTag = obj._addSeperator(angle, "");
    var number = numberTag.children().removeClass();
    number
        .addClass("rs-number")
        .html(`${i}&#176;`)
        .rsRotate(-(angle));
    }

    // // to set the path segment
    // const segment = obj.$path.getTotalLength() / 10;
    // obj.$path.style.strokeDasharray = [segment - 1, 1].join(" ");
    // // to set the path width
    }
}


const pInput = (value) => {
    emits("sliderValue", value);
}
</script>

<style>
span.rs-number {
  position: absolute;
  top: -10px;
  left: -35px;
  font-family: Segoe UI;
  font-size: 10px;
  font-weight: bold;
}
</style>
