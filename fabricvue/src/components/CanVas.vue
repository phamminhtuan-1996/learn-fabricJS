<template>
    <h1>{{ h1 }}</h1>
    
    <div class="wrap d-flex justify-content-center">
        <canvas ref="canVasSelector" width="500" height="500"></canvas>
        <ControlMenu
        @value-opacity="pickValueOpacity"
        @value-scale="pickValueScale"
        @value-blur="pickValueBlur"
        />
    </div>
</template>
<script lang="ts">
import { onMounted, ref } from 'vue';
import { fabric } from 'fabric';
import  ControlMenu  from './ControlMenu.vue';

export default {
    components: {
        ControlMenu,
    },
    setup() {
        const canVasSelector = ref<HTMLDivElement>();
        const h1 = ref<string>('CanVas To Fill');
        const valueOpacity = ref<number>(1.0);
        const canvasValue = ref();
        const rect = new fabric.Rect({
            fill: 'rgba(255, 0, 0, 1.0)',
            width: 150,
            height: 150,
            top: 10,
            left: 10,
        });

        const pickValueOpacity = (value: number) => {
            const obj = canvasValue.value.getActiveObject();
            valueOpacity.value = value;
            obj.set({
                opacity: valueOpacity.value,
            });
            console.log(obj);
            canvasValue.value.renderAll();
        };
        const pickValueScale = (value: number) => {
            let obj = canvasValue.value.getActiveObject();
             obj.set({
                width: value,
                height: value
            });
            canvasValue.value.renderAll();
        };
        const pickValueBlur = (value: number) => {
            let obj = canvasValue.value.getActiveObject();
            obj.set({
                blur: value,
            });
            canvasValue.value.renderAll();
        }
        onMounted(() => {
            const canvas = new fabric.Canvas(canVasSelector.value);
            canvasValue.value = canvas;
            canvasValue.value.add(rect);
            canvasValue.value.renderAll();
        });
        
        return {
            canVasSelector,
            h1,
            valueOpacity,
            pickValueOpacity,
            pickValueScale,
            pickValueBlur,
        };
    },
};
</script>
<style lang="scss" scoped>
.wrap {
    width: 100%;
    :deep(.canvas-container) {
        width: calc(70% - 5px);
        border: 1px solid black;
        margin-right: 5px;
    }
}
</style>
