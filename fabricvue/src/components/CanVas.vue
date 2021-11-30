<template>
    <h1>{{ h1 }}</h1>
    
    <div class="wrap d-flex justify-content-center">
        <canvas ref="canVasSelector" width="500" height="500"></canvas>
        <ControlMenu @value-opacity="pickValueOpacity"/>
    </div>
</template>
<script lang="ts">
import { onMounted, ref, watch } from 'vue';
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
            width: 50,
            height: 50,
            top: 10,
            left: 10,
        });
        const pickValueOpacity = (value: number) => {
            console.log(canvasValue.value);
            valueOpacity.value = value;
            rect.fill = `rgba(255, 0, 0, ${valueOpacity.value})`;
            canvasValue.value.renderAll();
        }
        onMounted(() => {
            const canvas = new fabric.Canvas(canVasSelector.value);
            canvasValue.value = canvas;
            canvasValue.value.add(rect);
            canvasValue.value.renderAll();
            console.log(canvasValue.value);
        });
        
        return {
            canVasSelector,
            h1,
            valueOpacity,
            pickValueOpacity,
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
