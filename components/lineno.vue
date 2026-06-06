<template>
    <div class="line-numbers">
      <pre><code><span v-for="(line, index) in lines" :key="index" class="line">
        <span class="line-number">{{ index + 1 }}</span>
        <span class="line-content" v-html="line"></span>
      </span></code></pre>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from 'vue';
  
  const contentRef = ref(null);
  const lines = ref([]);
  
  onMounted(() => {
    if (contentRef.value) {
      const content = contentRef.value.innerHTML;
      lines.value = content.split('\n').map(line => {
        return line.replace(/```(\w+)?|```/g, '')
                   .replace(/\[(.+?)\]\{\.(.+?)\}/g, '<span class="$2">$1</span>')
                   .replace(/\\\\/g, '\\') // Unescape backslashes
                   .trim();
      });
    }
  });
  </script>
  
  <style scoped>
  .line-numbers {
    font-family: monospace;
    white-space: pre;
    background-color: #f5f5f5;
    padding: 1em;
    border-radius: 4px;
  }
  .line {
    display: block;
    line-height: 1.5;
  }
  .line-number {
    display: inline-block;
    width: 2em;
    text-align: right;
    margin-right: 1em;
    color: #999;
    user-select: none;
  }
  .line-content {
    white-space: pre-wrap;
  }
  .pl-8 {
    padding-left: 2em;
  }
  .pl-16 {
    padding-left: 4em;
  }
  </style>