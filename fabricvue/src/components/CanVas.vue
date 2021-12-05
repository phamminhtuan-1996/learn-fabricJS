<template>
    <h1>{{ h1 }}</h1>
    
    <div class="wrap d-flex justify-content-center">
        <canvas ref="canVasSelector" width="500" height="500"></canvas>
        <ControlMenu
            :activeKeyAction="activeKeytab"
            :hasOpenRemove="isRemoveObj"
            :onDrop="dropEvent"
            @value-opacity="pickValueOpacity"
            @value-scale="pickValueScale"
            @value-blur="pickValueBlur"
            @add-text-canvas="pickValueText"
            @delete-obj-select="deleteObj"
            @create-shape="createShape"
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
        const checkActionObj = ref(false);
        const isRemoveObj = ref(true);
        const objTarget = ref();
        const activeKeytab = ref<number>(1);
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

        const pickValueText = (value: string) => {
            let createText = new fabric.Text(value, { left: 100, top: 100 });
            canvasValue.value.add(createText);
            canvasValue.value.renderAll();
            checkActionObj.value = !checkActionObj.value;
        }
        const toggleButtonRemove = (value: any) => {
            if(value) {
                isRemoveObj.value = false;
            } else {
                isRemoveObj.value = true;
            }
        }
        const deleteObj = () => {
            canvasValue.value.remove(objTarget.value);
            isRemoveObj.value = true;
        }
        const createShape = (value: string) => {
            switch (value) {
                case 'square':
                    let square = new fabric.Rect({
                        fill: 'blue',
                        width: 150,
                        height: 150,
                        top: 10,
                        left: 10,
                    });
                    canvasValue.value.add(square);
                    canvasValue.value.renderAll();
                    break;
                case 'triangle':
                    let triangle = new fabric.Triangle({
                        fill: 'violet',
                        width: 150,
                        height: 150,
                        top: 10,
                        left: 10,
                    });
                    canvasValue.value.add(triangle);
                    canvasValue.value.renderAll();
                    break;
                case 'circle':
                    let circle = new fabric.Circle({
                        left: 100,
                        top: 100,
                        radius: 50,
                        fill:'#f39c12'
                    });
                    circle.dirty = true;
                    canvasValue.value.add(circle);
                    canvasValue.value.renderAll();
                    break;
                case 'star':
                    let polygon = new fabric.Polygon([
                        {x: 200, y: 0},
                        {x: 250, y: 50},
                        {x: 250, y: 100},
                        {x: 150, y: 100},
                        {x: 150, y: 50} ], {
                            left: 250,
                            top: 150,
                            angle: 0,
                            fill: 'green'
                        }
                        );
                    canvasValue.value.add(polygon);
                    canvasValue.value.renderAll();
                    break;
            } 
            
        }
        const handleActiveTab = (value: number) => {
            activeKeytab.value = value;
        }
        const dropEvent = (value: any) => {
            console.log(value);
        }

        onMounted(() => {
            const canvas = new fabric.Canvas(canVasSelector.value);
            canvasValue.value = canvas;
            canvasValue.value.add(rect);
            canvasValue.value.renderAll();
            canvasValue.value.on('mouse:dblclick', (e: any) => {
                toggleButtonRemove(e.target);
                objTarget.value = e.target;
            });
        });
        
        return {
            canVasSelector,
            h1,
            valueOpacity,
            isRemoveObj,
            objTarget,
            activeKeytab,

            pickValueOpacity,
            pickValueScale,
            pickValueBlur,
            pickValueText,
            deleteObj,
            createShape,
            handleActiveTab,
            dropEvent,
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
