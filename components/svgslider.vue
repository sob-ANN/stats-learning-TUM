<template>
  <div :class="`svg-slider ${classes}`">
    <div ref="svgContainer" :class="`svg-container ${containerClasses}`"></div>
    <div :class="`controls ${controlsClasses}`">
      <button @click="prevImage" class="nav-button">◀</button>
      <input
        class="slider"
        type="range"
        :min="0"
        :max="svgs.length - 1"
        v-model="temporaryIndex"
        @input="updateTemporaryIndex"
        @change="updateCurrentIndex"
      />
      <button @click="nextImage" class="nav-button">▶</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch, toRefs } from "vue";

const props = defineProps({
  svgs: {
    type: Array,
    required: true,
  },
  classes: {
    type: String,
    default: '',
  },
  containerClasses: {
    type: String,
    default: '',
  },
  controlsClasses: {
    type: String,
    default: '',
  },
});

const { svgs } = toRefs(props);
const currentIndex = ref(0);
const temporaryIndex = ref(0);
const svgContainer = ref(null);

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

onMounted(() => {
  loadSvg(currentIndex.value);
});

watch(currentIndex, (newIndex) => {
  loadSvg(newIndex);
});
</script>

<style scoped>
.svg-slider {
  @apply flex flex-col items-center;
}
.controls {
  @apply flex flex-row items-center justify-between;
}
.controls input {
  margin: 0 10px;
}
.nav-button {
  @apply px-4 py-2 bg-gray-300 text-white rounded w-8 h-8 cursor-pointer flex items-center justify-center transition-colors;
}
.nav-button:hover {
  @apply bg-gray-400 transition-colors;
}
.slider {
  @apply w-72 h-8 mx-2 p-2 appearance-none rounded-md bg-gray-200 cursor-pointer outline-none opacity-70 transition-opacity;
}
.slider:hover {
  @apply opacity-100 transition-opacity;
}
.slider::-webkit-slider-thumb {
  @apply appearance-none w-6 h-6 bg-gray-400 rounded-md cursor-pointer;
}
.slider::-moz-range-thumb {
  @apply appearance-none w-6 h-6 bg-gray-400 rounded-md cursor-pointer;
}
</style>
