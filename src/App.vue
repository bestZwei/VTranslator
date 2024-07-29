<template>
  <div class="container">
    <h1>VTranslator</h1>
    <div v-if="isAuthenticated">
      <LanguageSelector @update-languages="updateLanguages" />
      <ModelSelector :models="models" v-model="selectedModel" />
      <TextArea v-model="inputText" placeholder="输入文本" />
      <button @click="translate">翻译</button>
      <button @click="copyToClipboard">复制</button>
      <TextArea v-model="outputText" placeholder="翻译结果" readonly />
      <div v-if="message" class="message">{{ message }}</div>
    </div>
  </div>
</template>

<script>
import LanguageSelector from './components/LanguageSelector.vue';
import ModelSelector from './components/ModelSelector.vue';
import TextArea from './components/TextArea.vue';

export default {
  components: {
    LanguageSelector,
    ModelSelector,
    TextArea,
  },
  data() {
    return {
      inputText: '',
      outputText: '',
      selectedModel: '',
      message: '',
      languages: [],
      models: process.env.MODELS.split(','),
      isAuthenticated: true, // 直接允许访问
    };
  },
  methods: {
    async translate() {
      const response = await fetch(process.env.PROXY_URL, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${process.env.API_KEY}`,
        },
        body: JSON.stringify({
          prompt: process.env.PROMPT.replace('{语言1}', this.languages.source).replace('{语言2}', this.languages.target),
          model: this.selectedModel,
          input: this.inputText,
        }),
      });
      const data = await response.json();
      if (data.success) {
        this.outputText = data.translatedText;
        this.message = '翻译成功！';
      } else {
        this.message = '翻译失败，请重试。';
      }
    },
    copyToClipboard() {
      navigator.clipboard.writeText(this.outputText).then(() => {
        this.message = '结果已复制！';
      }).catch(() => {
        this.message = '复制失败，请重试。';
      });
    },
    updateLanguages(languages) {
      this.languages = languages;
    },
  },
};
</script>