<template>
  <div id="app">
    <FormComponent/>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import FormComponent from "@/components/FormComponent.vue";

export default Vue.extend({
  name: 'App',
  components: {
    FormComponent,
  }
});
</script>

<style lang="scss" scoped>
#app {
  width: 100vw;
  height: 100vh;
  background: #f5f5f5;
  font-family: Arial;
}
</style>