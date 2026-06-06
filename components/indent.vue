<template>
  <span :style="indentStyle"></span>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  level: {
    type: Number,
    default: 1,
  },
  width: {
    type: Number,
    default: 20,
  },
});

const indentStyle = computed(() => ({
  display: "inline-block",
  width: `${props.level * props.width}px`,
}));
</script>
