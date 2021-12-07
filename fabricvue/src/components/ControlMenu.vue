<template>
<div class="menu-control d-flex flex-column card p-4">
    <a-tabs v-model:activeKey="activeKey" @change="changeTabIndex">
         <a-tab-pane key="1" tab="Menu Control">
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
                    <a-button class="w-100" type="primary" @click="groupForObj"><i class="far fa-object-group"></i></a-button>
                </a-col>
            </a-row>
         </a-tab-pane>
         <a-tab-pane key="2" tab="resources">
            <a-row :gutter="[16,16]">
                <a-col :span="8" v-for="(itemn, index) in 6" :key='index'>
                    <draggable v-model="myArray" group="people" @start="drag=true" @end="drag=false">
                        <img ref="imgDrag" class="w-100" :draggable="true" :ondragstart="drag($event)" src="https://i.pinimg.com/564x/53/b4/81/53b48189e18cf1504a27a39d7b66a4d0.jpg" alt="">
                    </draggable>
                </a-col>
            </a-row>
         </a-tab-pane>
         <a-tab-pane key="3" tab="layer" class="d-flex flex-column">
            <div
                class="layer-item w-100 d-flex justify-content-between align-items-center my-2"
                v-for="(item, index) in listLayer" :key="index"
                @click="selectObject(item)"
             >
             <a-button>
                 <span>{{ item.text || item.fill }}</span>
             </a-button>
             <i class="fas fa-folder-minus" @click="unGroupObj"></i>
              <i class="fas fa-trash" @click="removeObj(item)"></i>
              </div>
         </a-tab-pane>
    </a-tabs>
</div>
</template>
<script lang="ts">
import { ref, onMounted, watch } from 'vue';
import draggable from 'vuedraggable'

export default {
    conponents: {
        draggable,
    },
    props: {
        hasOpenRemove: {
            type: Boolean,
            default: true
        },
        listLayer: {
            type: Object,
            required: true
        },
        isActionCanvas: {
            type: Boolean,
            required: true
        }
    },
  setup(props: any, { emit }: any) {
    const inputValue = ref<number>(0);
    const inputValue1 = ref<number>(1);
    const inputValue2 = ref<number>(150);
    const inputValue3 = ref<number>(150);
    const valueTextObj = ref<string>('');
    const isShowShape = ref<boolean>(false);
    const imgDrag = ref<HTMLDivElement>();
    const layerList = ref(props.listLayer);
    const shapeList = ref<any[]>([
        { shape: 'fa-square', value: 'square' },
        { shape: 'fa-caret-up fs-3',  value: 'triangle' },
        { shape: 'fa-circle',  value: 'circle' },
        { shape: 'fa-star',  value: 'star' },
    ]);
    const activeKey = ref('1');
    const listObjectsLayer = ref<any[]>([]);
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
    const removeObj = (item: any) => {
         emit('delete-obj-select', item);
    }
    const toggleListShape = () => {
        isShowShape.value = !isShowShape.value;
    }
    const selectCreateShape = (value: string) => {
         emit('create-shape', value);
    }
    const drag = (event: any) => {
        console.log('drag', event);
    } 
    const changeTabIndex = (item: any) => {
        console.log()
        emit('change-index-tab', item);
    }
    const selectObject = (item: any) => {
        emit('active-object-value', item);
    }
    const groupForObj = () => {
        emit('group-for-object');
    }
    const unGroupObj = () => {
        emit('ungrou-for-object');
    }
    watch(() => props.hasOpenRemove, (value: boolean) => {
        console.log('da doi dieu kien', value);
        console.log('activeKey = ', activeKey.value);
        if(activeKey.value === '2' && !value) {
            activeKey.value = '1';
        }
    });
    watch(() => props.isActionCanvas, () => {
       setTimeout(() => {
            console.log('list layer control menu', props.listLayer);
       }, 500);
    });
    return {
      inputValue,
      inputValue1,
      inputValue2,
      inputValue3,
      valueTextObj,
      isShowShape,
      shapeList,
      imgDrag,
      layerList,
      activeKey,
      listObjectsLayer,

      handleSlider,
      handleSliderScale,
      handleSliderBlur,
      handleInputText,
      removeObj,
      toggleListShape,
      selectCreateShape,
      drag,
      changeTabIndex,
      selectObject,
      groupForObj,
      unGroupObj,
    };
  },
};
</script>
<style lang="scss" scoped>
.menu-control {
    width: calc(30% - 10px);
    margin-left: 10px;
    img {
        height: 100px;
        object-fit: cover;
    }
}
</style>
