<template>
<div class="menu-control d-flex flex-column card p-4">
    <h1 class="display-3">menu</h1>
    <h3 class="text-start">opacity</h3>
    <a-row> 
        <a-col :span="12">
            <a-slider v-model:value="inputValue1" :min="0.1" :max="1.0" :step="0.1" @change="handleSlider" />
         </a-col>
        <a-col :span="4">
            <a-input-number v-model:value="inputValue1" :min="0.1" :max="1.0" :step="0.1" style="margin-left: 16px" @change="handleSlider"/>
        </a-col>
    </a-row>
    <h3 class="text-start">scale</h3>
    <a-row> 
        <a-col :span="12">
            <a-slider v-model:value="inputValue2" :min="50" :max="150" @change="handleSliderScale" />
         </a-col>
        <a-col :span="4">
            <a-input-number v-model:value="inputValue2" :min="50" :max="150" style="margin-left: 16px" @change="handleSliderScale"/>
        </a-col>
    </a-row>
    <h3 class="text-start">blur</h3>
    <a-row> 
        <a-col :span="12">
            <a-slider v-model:value="inputValue3" :min="1" :max="70" @change="handleSliderBlur" />
         </a-col>
        <a-col :span="4">
            <a-input-number v-model:value="inputValue3" :min="1" :max="70" style="margin-left: 16px" @change="handleSliderBlur"/>
        </a-col>
    </a-row>
</div>
</template>
<script lang="ts">
import { ref } from 'vue';

export default {
  setup(props: any, { emit }: any) {
    const inputValue = ref<number>(0);
    const inputValue1 = ref<number>(1);
    const inputValue2 = ref<number>(150);
    const inputValue3 = ref<number>(150);

    const handleSlider = (value: number) => {
        inputValue.value = value;
        inputValue1.value = value;
        emit('value-opacity', value);
    };
    const handleSliderScale = (value: number) => {
      inputValue2.value = value;
      emit('value-scale', value);
    }

    const handleSliderBlur = (value: number) => {
      inputValue3.value = value;
      emit('value-blur', value);
    }

    return {
      inputValue,
      inputValue1,
      inputValue2,
      inputValue3,
      handleSlider,
      handleSliderScale,
      handleSliderBlur
    };
  },
};
</script>
<style lang="scss" scoped>
.menu-control {
    width: calc(30% - 10px);
    margin-left: 10px;
}
</style>
