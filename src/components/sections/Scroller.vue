<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const offsetX = ref(0)
const activeIndex = ref(0)
const container = ref(null)
const contentWidth = ref(0)
const items = [
  {
    image: '/image_slider/huaqiang/input/0.png'
  },
  {
    image: '/image_slider/huaqiang/input/1.png'
  }
] // 你的图片数据


// 物理参数
const stiffness = 0.2
const damping = 0.8
const minVelocity = 0.1
let animationId = null
let velocity = 0
let lastX = 0
let isAnimating = false

const initSize = () => {
  if (container.value) {
    contentWidth.value = container.value.offsetWidth * items.value.length
  }
}

// 边界限制函数
const applyBounds = () => {
  const maxOffset = 0
  const minOffset = -(contentWidth.value - container.value.offsetWidth)

  if (offsetX.value > maxOffset) {
    offsetX.value = maxOffset
    velocity = 0
  }
  if (offsetX.value < minOffset) {
    offsetX.value = minOffset
    velocity = 0
  }
}

const animate = () => {
  if (Math.abs(velocity) > minVelocity) {
    velocity *= damping
    offsetX.value += velocity
    applyBounds()
    animationId = requestAnimationFrame(animate)
  } else {
    isAnimating = false
    snapToNearest()
  }
}

// 触摸滑动处理
let startX = 0
let startOffset = 0
let touchTime = 0

const handleTouchStart = (e) => {
  cancelAnimationFrame(animationId)
  isAnimating = false
  startX = e.touches[0].clientX
  startOffset = offsetX.value
  touchTime = Date.now()
}

const handleTouchMove = (e) => {
  const currentX = e.touches[0].clientX
  const delta = currentX - startX
  offsetX.value = startOffset + delta
  applyBounds()

  // 计算实时速度
  velocity = (delta / (Date.now() - touchTime)) * 16
  touchTime = Date.now()
}

const handleTouchEnd = () => {
  isDragging = false
  snapToNearest()
}

// 惯性滑动
let momentum = 0
let lastTime = 0

const handleWheel = (e) => {
  const now = Date.now()
  const delta = e.deltaX || e.deltaY
  momentum = delta * (now - lastTime) / 16
  lastTime = now
  requestAnimationFrame(animateMomentum)
}

const animateMomentum = () => {
  if (Math.abs(momentum) > 0.5) {
    offsetX.value += momentum
    momentum *= 0.95
    requestAnimationFrame(animateMomentum)
  }
}

// 吸附到最近项
const snapToNearest = () => {
  const containerWidth = container.value.offsetWidth
  const targetIndex = Math.round(-offsetX.value / containerWidth)
  activeIndex.value = Math.max(0, Math.min(targetIndex, items.length - 1))
  offsetX.value = -activeIndex.value * containerWidth
}

onMounted(() => {
  container.value.addEventListener('touchstart', handleTouchStart)
  container.value.addEventListener('touchmove', handleTouchMove)
  container.value.addEventListener('touchend', handleTouchEnd)
  container.value.addEventListener('wheel', handleWheel)
})

onUnmounted(() => {
  // 清理事件监听
})
</script>

<template>
  <div class="scroll-container" ref="container">
    <div class="scroll-content" :style="{ transform: `translateX(${offsetX}px)` }">
      <div
          v-for="(item, index) in items"
          :key="index"
          class="slide-item"
          :class="{ 'active': activeIndex === index }"
          @click="handleItemClick(index)"
      >
        <img :src="item.image" alt="">
      </div>
    </div>
  </div>
</template>

<style scoped>
.slide-item {
  position: relative;
  border-radius: 12px;
  overflow: hidden;
  transition: transform 0.3s ease;

  /* 外层光晕 */
  &::before {
    content: '';
    position: absolute;
    inset: -2px;
    background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
    z-index: -1;
    border-radius: 14px;
    filter: blur(8px);
    opacity: 0.5;
    transition: opacity 0.3s;
  }

  /* 内层边框 */
  &::after {
    content: '';
    position: absolute;
    inset: 0;
    border: 2px solid rgba(255,255,255,0.2);
    border-radius: 12px;
    pointer-events: none;
  }

  &:hover {
    transform: translateY(-5px);

    &::before {
      opacity: 0.8;
    }
  }
}
</style>