<template>
<div class="menu-control d-flex flex-column card p-4">
    <h1 class="display-3">menu</h1>
    <h3 class="text-uppercase text-start">opacity</h3>
    <a-row> 
        <a-col :span="12">
            <a-slider v-model:value="inputValue1" :min="0.1" :max="1.0" :step="0.1" @change="handleSlider" />
         </a-col>
        <a-col :span="4">
            <a-input-number v-model:value="inputValue1" :min="0.1" :max="1.0" :step="0.1" style="margin-left: 16px" @change="handleSlider"/>
        </a-col>
    </a-row>
    <h3 class="text-uppercase text-start">scale</h3>
    <a-row> 
        <a-col :span="12">
            <a-slider v-model:value="inputValue2" :min="50" :max="150" @change="handleSliderScale" />
         </a-col>
        <a-col :span="4">
            <a-input-number v-model:value="inputValue2" :min="50" :max="150" style="margin-left: 16px" @change="handleSliderScale"/>
        </a-col>
    </a-row>
    <h3 class="text-uppercase text-start">blur</h3>
    <a-row> 
        <a-col :span="12">
            <a-slider v-model:value="inputValue3" :min="1" :max="70" @change="handleSliderBlur" />
         </a-col>
        <a-col :span="4">
            <a-input-number v-model:value="inputValue3" :min="1" :max="70" style="margin-left: 16px" @change="handleSliderBlur"/>
        </a-col>
    </a-row>
    <h3 class="text-uppercase text-start">add text</h3>
     <a-row :gutter="[16,16]"> 
        <a-col :span="12">
            <a-input v-model:value="valueTextObj" placeholder="Basic usage" @keypress.enter="handleInputText" />
         </a-col>
        <a-col :span="4">
            <a-button class="text-uppercase" type="primary" @click="handleInputText">add text</a-button>
        </a-col>
    </a-row>
    <a-row class="mt-4" :gutter="[16,16]">
        <a-col :span="24">
            <a-button class="w-100 d-flex justify-content-center align-items-center" type="primary" @click="toggleListShape"> <span class="text-uppercase d-block mx-2">create shape</span> <i class="fas fa-shapes"></i></a-button>
        </a-col>
        <a-col :span="24" v-if="isShowShape">
             <a-row>
                <a-col :span="6" v-for="(itemn, index) in shapeList" :key="index">
                    <a-button type="primary" @click="selectCreateShape(itemn.value)"><i class="fas" :class="itemn.shape"></i></a-button>
                </a-col>
             </a-row>
        </a-col>
        <a-col :span="24">
            <a-button class="w-100" type="primary" danger :disabled="hasOpenRemove" @click="removeObj"><i class="fas fa-trash"></i></a-button>
        </a-col>
    </a-row>
</div>
</template>
<script lang="ts">
import { ref, onMounted } from 'vue';

export default {
    props: {
        hasOpenRemove: {
            type: Boolean,
            default: true
        }
    },
  setup(props: any, { emit }: any) {
    const inputValue = ref<number>(0);
    const inputValue1 = ref<number>(1);
    const inputValue2 = ref<number>(150);
    const inputValue3 = ref<number>(150);
    const valueTextObj = ref<string>('');
    const isShowShape = ref<boolean>(false);
    const shapeList = ref<any[]>([
        { shape: 'fa-square', value: 'square' },
        { shape: 'fa-caret-up fs-3',  value: 'triangle' },
        { shape: 'fa-circle',  value: 'circle' },
        { shape: 'fa-star',  value: 'star' },
    ]);

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
    const handleInputText = () => {
      emit('add-text-canvas', valueTextObj.value);
      valueTextObj.value = '';
    };
    const removeObj = () => {
         emit('delete-obj-select');
    }
    const toggleListShape = () => {
        isShowShape.value = !isShowShape.value;
    }
    const selectCreateShape = (value: string) => {
         emit('create-shape', value);
    }
    return {
      inputValue,
      inputValue1,
      inputValue2,
      inputValue3,
      valueTextObj,
      isShowShape,
      shapeList,

      handleSlider,
      handleSliderScale,
      handleSliderBlur,
      handleInputText,
      removeObj,
      toggleListShape,
      selectCreateShape,
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
