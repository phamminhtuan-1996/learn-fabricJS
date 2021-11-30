

<template>
  <CanVas />
  <ControlMenu />
</template>
<script lang="ts">
import CanVas from './components/CanVas.vue';
export default {
  setup() {

  },
  components: {
    CanVas,
  },
};
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
