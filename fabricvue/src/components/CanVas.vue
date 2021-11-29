<template>
    <h1>{{ h1 }}</h1>
    <canvas ref="canVas" width="200" height="200"></canvas>
</template>
<script lang="ts">
import { ref } from 'vue';
import { fabric } from 'fabric';

export default {
    setup() {
        const canVas = ref();
        const h1 = ref<string>('CanVas To Fill');
        const canvas = new fabric.Canvas(canVas);
        console.log(canvas);
        return {
            h1,
        };
    },
};
</script>
