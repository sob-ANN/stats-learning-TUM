<template>
  <div :class="`svg-slider ${classes}`">
    <div ref="svgContainer" class="svg-container"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch, toRefs, onUnmounted } from "vue";

const props = defineProps({
  svgs: {
    type: Array,
    required: true,
  },
  classes: {
    type: String,
    default: '',
  },
});

const { svgs } = toRefs(props);
const currentIndex = ref(0);
const temporaryIndex = ref(0);
const svgContainer = ref(null);
let intervalId = null;

const loadSvg = async (index) => {
  if (!svgContainer.value) return;
  console.log(`Loading SVG at index: ${index}`);
  try {
    console.log(`Appending SVG element for index: ${index}`);
    const response = await fetch(svgs.value[index]);
    if (!response.ok) {
      throw new Error(`Failed to fetch SVG: ${response.statusText}`);
    }
    const svgText = await response.text();
    svgContainer.value.innerHTML = svgText;
  } catch (error) {
    console.error("Error loading SVG:", error);
  }
};

const prevImage = () => {
  if (currentIndex.value > 0) {
    currentIndex.value -= 1;
    temporaryIndex.value = currentIndex.value; // Sync temporaryIndex
    console.log(`Prev image, new index: ${currentIndex.value}`);
  }
};

const nextImage = () => {
  if (currentIndex.value < svgs.value.length - 1) {
    currentIndex.value += 1;
    temporaryIndex.value = currentIndex.value; // Sync temporaryIndex
    console.log(`Next image, new index: ${currentIndex.value}`);
  } else {
    currentIndex.value = 0; // Loop back to the first image
    temporaryIndex.value = currentIndex.value; // Sync temporaryIndex
    console.log(`Looping to first image, new index: ${currentIndex.value}`);
  }
};

const updateTemporaryIndex = (event) => {
  temporaryIndex.value = Number(event.target.value); // Ensure the value is a number
  console.log(`Updating temporary index to: ${temporaryIndex.value}`);
};

const updateCurrentIndex = () => {
  currentIndex.value = Number(temporaryIndex.value); // Ensure the value is a number
  console.log(`Updating current index to: ${currentIndex.value}`);
};

const startAutoLoop = () => {
  intervalId = setInterval(() => {
    nextImage();
  }, 500);
};

const stopAutoLoop = () => {
  if (intervalId) {
    clearInterval(intervalId);
  }
};

onMounted(() => {
  loadSvg(currentIndex.value);
  startAutoLoop();
});

onUnmounted(() => {
  stopAutoLoop();
});

watch(currentIndex, (newIndex) => {
  loadSvg(newIndex);
});
</script>

<style scoped>
.svg-slider {
  @apply flex flex-col items-center;
}
.svg-container {
  @apply w-145;
}
</style>
