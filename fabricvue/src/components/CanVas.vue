<template>
    <h1>{{ h1 }}</h1>
    
    <div class="wrap d-flex justify-content-center">
        <canvas ref="canVasSelector" width="500" height="500"></canvas>
        <ControlMenu />
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
