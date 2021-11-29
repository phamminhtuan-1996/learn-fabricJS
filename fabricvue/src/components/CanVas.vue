<template>
    <h1>{{ h1 }}</h1>
    <div>
        <canvas ref="canVasSelector" width="500" height="500"></canvas>
    </div>
</template>
<script lang="ts">
import { onMounted, ref } from 'vue';
import { fabric } from 'fabric';

export default {
    setup() {
        const canVasSelector = ref<HTMLDivElement>();
        const h1 = ref<string>('CanVas To Fill');
        onMounted(() => {
            const canvas = new fabric.Canvas(canVasSelector.value);
            const rect = new fabric.Rect({
                fill: 'red',
                width: 50,
                height: 50,
                top: 10,
                left: 10,
            });
            canvas.add(rect);
            canvas.renderAll();
        });
        return {
            canVasSelector,
            h1,
        };
    },
};
</script>
