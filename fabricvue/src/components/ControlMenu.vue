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
</div>
</template>
<script lang="ts">
import { ref, watch } from 'vue';

export default {
  setup() {
    const inputValue = ref<number>(0);
    const inputValue1 = ref<number>(1);
    const handleSlider = (value: number) => {
        inputValue.value = value;
        inputValue1.value = value;
    }
    watch(() => inputValue.value, (value: number)=> {
        console.log(value);
    });
    return {
      inputValue,
      inputValue1,
      handleSlider,
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
