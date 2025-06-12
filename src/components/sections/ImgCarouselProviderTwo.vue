<script setup>
import { Swiper, SwiperSlide } from 'swiper/vue';
import { Navigation, Pagination, Autoplay } from 'swiper/modules';
import 'swiper/css/bundle';
import { ref, onMounted, nextTick } from "vue";

const modules = [Navigation, Pagination, Autoplay];

const maleList = [
  "/showcase/images/m_3.jpg",
  "/showcase/images/m_1.png",
  "/showcase/images/m_2.jpg",
  "/showcase/images/m_13.jpg",
  "/showcase/images/m_8.png",
  "/showcase/images/m_11.png",
];

const femaleList = [
  "/showcase/images/f_14.jpg",
  "/showcase/images/f_0_w.png",
  "/showcase/images/f_1.png",
  "/showcase/images/f_2_w.png",
  "/showcase/images/f_9.jpg",
  "/showcase/images/f_5_w.png",

];

const image_paths = [...maleList, ...femaleList];


const isDragging = ref(false);
const dragImage = ref(null);
const swiperRef = ref(null);
const currentHoverIndex = ref(null); // 记录当前悬浮的 index
const draggingIndex = ref(null);

const createDragImage = () => {
  const img = new Image();
  // img.src = "./carousel/1.jpg";
  img.style.width = "100px";
  img.style.height = "100px";
  img.style.position = "absolute";
  img.style.top = "-9999px"; // 隐藏但仍在 DOM 中
  document.body.appendChild(img);

  // img.onload = () => {
  //   dragImage.value = img;
  // };
  dragImage.value = img;
};

// emit part
import { defineEmits } from 'vue';

const emit = defineEmits(['mouseenter', 'mouseleave', 'dragstart', 'dragend', "updateDraggingIndex"]);


// **鼠标悬浮时，提前更新拖动图片**
const handleMouseEnter = (index) => {
  currentHoverIndex.value = index;
  emit('mouseenter');
  if (dragImage.value) {
    dragImage.value.src = image_paths[index]; // 先修改 `src`
  }
};

const handleMouseLeave = (index) => {

  emit('mouseleave');

};

const handleDragStart = (event, index) => {
  console.log(`开始拖动图像，当前图像索引: ${index}`);
  isDragging.value = true;
  draggingIndex.value = index; // 记录拖动的索引
  emit('dragstart');
  emit("updateDraggingIndex", index); // 发送更新索引的事件

  dragImage.value.src = image_paths[index];

  if (dragImage.value) {
    event.dataTransfer.setDragImage(dragImage.value, 50, 50);
  }
  // dragImage.value.onload = () => {
  //   event.dataTransfer.setDragImage(dragImage.value, 50, 50);
  // };

  nextTick(() => {
    if (swiperRef.value?.swiper) {
      swiperRef.value.swiper.autoplay.stop();
    }
  });
};

const handleDragEnd = (index) => {
  console.log(`拖动结束，当前图像索引: ${index}`);
  isDragging.value = false;
  draggingIndex.value = null; // 重置索引
  emit('dragend');
  emit("updateDraggingIndex", null); // 通知父组件清空索引
  nextTick(() => {
    if (swiperRef.value?.swiper) {
      swiperRef.value.swiper.autoplay.start();
    }
  });
};

onMounted(() => {
  createDragImage();
});



</script>

<template>
  <el-divider />
  <el-row justify="center">
    <h1 class="section-title"> Showcase </h1>
  </el-row>

  <el-row justify="center">
    <el-col :xs="20" :sm="18" :md="16" :lg="16" :xl="16">
      <!-- 设置轮播图：循环播放、首张图序号、响应式、导航和分页、自动播放 -->
      <div class="swiper-container">
        <swiper
          :loop="true"
          :slidesPerView="1"
          :breakpoints="{
            400: {
              slidesPerView: 1,
            },
            600: {
              slidesPerView: 2,
            },
            800: {
              slidesPerView: 3,
            },
          }"
            :modules="modules"
            :navigation="{
            hideOnClick:true,
          }"
            :pagination="{
            hideOnClick:true,
            clickable:true,
            type:'bullets'
          }"
            :autoplay="{
            delay:5000,
            disableOnInteraction:false,
            pauseOnMouseEnter:true,
          }"
          :touchStartPreventDefault="false"
        >
          <swiper-slide v-for="(path, index) in maleList" :key="index">
            <!-- 添加拖动事件处理 -->
            <el-image
                :src="path" class="swiper"
                :alt="'image-' + index"
                draggable="true"
                @dragstart="handleDragStart($event, index)"
                @dragend="handleDragEnd(index)"
                @mouseenter="handleMouseEnter(index)"
                @mouseleave="handleMouseLeave(index)"
            />
            <div class="border-effect"></div>
          </swiper-slide>
        </swiper>

        <!-- 右侧 swiper（直接复制）-->
        <swiper
            :loop="true"
            :slidesPerView="1"
            :breakpoints="{
            400: {
              slidesPerView: 1,
            },
            600: {
              slidesPerView: 2,
            },
            800: {
              slidesPerView: 3,
            },
          }"
            :modules="modules"
            :navigation="{
            hideOnClick:true,
          }"
            :pagination="{
            hideOnClick:true,
            clickable:true,
            type:'bullets'
          }"
            :autoplay="{
            delay:5000,
            disableOnInteraction:false,
            pauseOnMouseEnter:true,
          }"
            :touchStartPreventDefault="false"
        >
          <swiper-slide v-for="(path, index) in femaleList" :key="index">
            <!-- 添加拖动事件处理 -->
            <el-image
                :src="path" class="swiper"
                :alt="'image-' + (index + maleList.length)"
                draggable="true"
                @dragstart="handleDragStart($event, index + maleList.length)"
                @dragend="handleDragEnd(index + maleList.length)"
                @mouseenter="handleMouseEnter(index + maleList.length)"
                @mouseleave="handleMouseLeave(index + maleList.length)"
            />
            <div class="border-effect"></div>
          </swiper-slide>
        </swiper>
      </div>
    </el-col>
  </el-row>
</template>

<style>
.swiper-container {
  display: flex; /* 让两个 swiper 在同一行 */
  justify-content: space-between; /* 让它们平均分布 */
  gap: 20px; /* 控制间距 */
  height: auto;
  width: 100%;
  padding-top: 20px;
}

/* 设置Swiper风格 */
.swiper {
  --swiper-theme-color: #424242;
  border-radius: 10px;
  margin: 5px;
  filter: drop-shadow(0 0 8px rgba(248, 248, 248, 0.3));
  flex: 1;
  min-width: 0;  /* 防止缩小 */
  max-width: 100%;
  height: 100%;
}

.swiper:hover {
  transform: translateY(-3px) scale(1.05);
  box-shadow: 0px 0px 20px rgba(172, 172, 172, 0.6); /* 添加光圈 */

  .swiper:before {
    opacity: 0.8;
  }
}

.border-effect {
  position: absolute;
  inset: 0;

  border-image: linear-gradient(45deg, #fff, rgba(255,255,255,0.5)) 1;
  border: 5px;
  mask:
      linear-gradient(#fff 1 1) content-box,
      linear-gradient(#fff 1 1);
  mask-composite: exclude;
  filter: drop-shadow(0 0 8px rgba(255,255,255,0.8));
}

.swiper:hover + .border-effect {
  box-shadow: 0px 0px 30px rgba(255, 255, 255, 0.8); /* 悬停时光圈变强 */
}


</style>
