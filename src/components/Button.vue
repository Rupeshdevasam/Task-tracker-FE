<template>
  <button @click="onClick" :style="{background: color}" class="btn">{{ text }}</button>
</template>

<script>
export default {
  name: "Button",
  props: {
    text: {
      type: String,
      default: "Click me",
    },
    color: String,
  },
  methods:{
      onClick(){
          this.$emit('clicked');
      }
  }
};
</script>

<style scoped>
</style>