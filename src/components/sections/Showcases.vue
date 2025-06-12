<script setup>
// import ImgCarouselProvider from './ImgCarouselProvider.vue'
import ImgCarouselProviderTwo from './ImgCarouselProviderTwo.vue'
import GSReceiver from './GSReceiver.vue'


// control interaction between child components
import { ref } from 'vue';
const showAddIcon = ref(false);
const isFlashing = ref(false);
const draggingIndex = ref(null); // 用于存储当前拖拽的索引

const handleMouseEnter = () => {
  console.log('mouseenter in parent');
  showAddIcon.value = true;
};

const handleMouseLeave = () => {
  console.log('mouseleave in parent');
  showAddIcon.value = false;
};

const handleDragStart = () => {
  console.log('dragstart in parent');
  isFlashing.value = true;
};

const handleDragEnd = () => {
  console.log('dragend in parent');
  isFlashing.value = false;
};

// 更新 draggingIndex
const updateDraggingIndex = (index) => {
  console.log(`更新拖拽索引: ${index}`);
  draggingIndex.value = index;
};


</script>

<template>
  <ImgCarouselProviderTwo
      @mouseenter="handleMouseEnter"
      @mouseleave="handleMouseLeave"
      @dragstart="handleDragStart"
      @dragend="handleDragEnd"
      @updateDraggingIndex="updateDraggingIndex"
  />

  <GSReceiver
      :showAddIcon="showAddIcon"
      :isFlashing="isFlashing"
      :draggingIndex="draggingIndex"
  />
</template>