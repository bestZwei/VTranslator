<template>
  <div class="model-selector">
    <select v-model="selectedModel">
      <option v-for="model in models" :key="model" :value="model">{{ model }}</option>
    </select>
  </div>
</template>

<script>
export default {
  props: ['models'],
  data() {
    return {
      selectedModel: '',
    };
  },
  watch: {
    selectedModel(newModel) {
      this.$emit('input', newModel);
    },
  },
};
</script>