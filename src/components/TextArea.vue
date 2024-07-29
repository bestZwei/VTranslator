<template>
  <textarea v-model="value" :placeholder="placeholder" :readonly="readonly"></textarea>
</template>

<script>
export default {
  props: ['value', 'placeholder', 'readonly'],
};
</script>