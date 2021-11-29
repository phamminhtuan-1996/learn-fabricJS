<template>
    <h1>{{ h1 }}</h1>
    <div ref="canVas">
        <canvas  width="200" height="200"></canvas>
    </div>
</template>
<script lang="ts">
import { ref } from 'vue';
import { fabric } from 'fabric';

export default {
    setup() {
        const canVasSelector = ref<HTMLDivElement>();
        const canvasSelectorDetails = canVasSelector.value?.querySelector('canvas');
        const h1 = ref<string>('CanVas To Fill');
        console.log(canvasSelectorDetails);
        const canvas = new fabric.Canvas(canvasSelectorDetails);
        const rect = new fabric.Rect({
            fill: 'red',
            width: 20,
            height: 20
        });
        canvas.add(rect);
        return {
            canVasSelector,
            h1,
            canvasSelectorDetails,
            canvas,
            rect,
        };
    },
};
</script>
