<template>
  <div class="language-selector">
    <select v-model="sourceLanguage">
      <option v-for="lang in languageOptions" :key="lang.code" :value="lang.code">{{ lang.name }}</option>
    </select>
    <select v-model="targetLanguage">
      <option v-for="lang in languageOptions" :key="lang.code" :value="lang.code">{{ lang.name }}</option>
    </select>
    <button @click="updateLanguages">确定选择</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      sourceLanguage: '',
      targetLanguage: '',
      languageOptions: [
        { code: 'zh', name: '中文' },
        { code: 'de', name: '德语' },
        { code: 'en', name: '英语' },
        { code: 'fr', name: '法语' },
        { code: 'ja', name: '日语' },
      ],
    };
  },
  methods: {
    updateLanguages() {
      this.$emit('update-languages', {
        source: this.sourceLanguage,
        target: this.targetLanguage,
      });
    },
  },
};
</script>