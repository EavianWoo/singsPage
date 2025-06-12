<script setup lang="ts">
import { onMounted, onBeforeUnmount } from 'vue';
import { ref, watch, defineProps, nextTick } from 'vue';
import * as GaussianSplats3D from '@mkkellogg/gaussian-splats-3d';


const viewers = ref<GaussianSplats3D.Viewer[]>([]);
const gsContainers = ref<(HTMLElement | null)[]>([]);
const hasInitialized = ref<boolean[]>([]); // 记录哪些 viewer had been initialized
// const isDragging = ref<boolean>(false);
// import * as THREE from 'three';



const props = defineProps(['showAddIcon', 'isFlashing', 'draggingIndex']);
const addIconRefs = ref<(HTMLElement | null)[]>([]);

const selectedSplatIndex = ref(null);


// show and animate
watch([() => props.showAddIcon, () => props.isFlashing], ([showAddIcon, isFlashing]) => {
  addIconRefs.value.forEach((icon) => {
    if (icon) {
      icon.style.display = showAddIcon ? 'flex' : 'none';
      icon.classList.toggle('flashing', isFlashing);
    }
  });
});

watch(() => props.draggingIndex, (newIndex) => {
  if (newIndex !== null) {  // update only if newIndex is not null
    console.log(`GaussianSplats3D received draggingIndex: ${newIndex}`);
    selectedSplatIndex.value = newIndex;
  }
});


// whether within overlay
const isOverZone = ref(false);

// 获取高斯组件的边界
const handleDragOver = (event) => {
  event.preventDefault(); // 阻止默认行为
  const overlay = event.target.closest('.overlay');  // 获取目标 overlay 元素
  if (overlay) {
    const { top, left, width, height } = overlay.getBoundingClientRect();  // 获取 overlay 的 clientRect
    const isInBounds =
        event.clientX >= left &&
        event.clientX <= left + width &&
        event.clientY >= top &&
        event.clientY <= top + height;

    // 如果在 overlay 区域内，设置背景为红色
    isOverZone.value = true;
    if (isInBounds) {
      overlay.style.style = 'block'
      // overlay.style.background = 'rgba(255, 0, 0, 0.5)'; // 红色背景
      overlay.style.border = '2px dashed rgba(255, 0, 0, 0.5)'; // 添加 2px 红色半透明边框
      overlay.style.borderRadius = '10px'; // 圆角
      overlay.style.background = 'transparent'; // 确保背景不变色
    }
  }
  else {

  }

};

const handleDragLeave = (event) => {
  event.preventDefault();
  const overlay = event.target.closest('.overlay');
  if (overlay) {
    overlay.style.background = 'rgba(0, 0, 0, 0)';
    overlay.style.border = '';
    overlay.style.borderRadius = '';
  }
}

const handleDrop = (event) => {
  event.preventDefault();
  const overlay = event.target.closest('.overlay');
  if (overlay) {
    overlay.style.background = 'rgba(0, 0, 0, 0.0)'; // 恢复背景色
  }
  if (isOverZone.value) {
    console.log("----> 图片被放置在gs区域");
    overlay.style.style = 'none'
  }

  const gsContainerIndex = gsContainers.value.findIndex((container) => container && container.contains(event.target));

  if (gsContainerIndex === -1) return;
  console.log('gsContainer, ', gsContainerIndex);

  if (isOverZone.value) {
    console.log('[selected_index]:', selectedSplatIndex.value)

    if (selectedSplatIndex.value !== null && selectedSplatIndex.value < splatFiles.value.length) {
      // update splat scene

      const viewer = viewers.value[gsContainerIndex];
      console.log('viewer', viewer);
      if (!viewer) return;

      // if (splatFilesInViewers.value[gsContainerIndex]) {
      //   console.log(`[Removing old splat] ${splatFilesInViewers.value[gsContainerIndex]}`);
      //   viewer.removeSplatScene(0);
      // }

      splatFilesInViewers[gsContainerIndex] = selectedSplatIndex.value;
      // console.log(`[Adding new splat] ${item.path}`);
      // viewer.addSplatScene(item.path, {
      //   splatAlphaRemovalThreshold: item.splatAlphaRemovalThreshold,
      //   showLoadingUI: item.showLoadingUI,
      //   position: item.position,
      //   rotation: item.rotation,
      //   scale: item.scale,
      // })
      //     .then(() => {
      //       viewer.start()
      //     })

      splatFilesInViewers.value[gsContainerIndex] = splatFiles.value[selectedSplatIndex.value];

      console.log('splatFilesInViewers[gsContainerIndex] was set to ', splatFilesInViewers.value[gsContainerIndex]);

    }

    console.log('[drop in viewer]');

  }

};


// TODO 这里改成相viewerOpts控制参数
const showCases = ref([
  {
    path: '/showcase/3dgs/m_1_new.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 1, -1],
    rotation: [0, 0, 0, 1],
    scale: [0.18, 0.18, 0.18]
  },
  {
    path:'/3dgs/converted.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: false,
    position: [3, 5, 5],
    rotation: [0, 1, 0, 1],
    scale: [0.5, 0.5, 0.5],
  },
  {
    path: '/showcase/3dgs/m_1.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: false,
    position: [0, 0, -0.5],
    rotation: [1, 0, 0, 0],
    scale: [0.5, 0.5, 0.5],
  },
  {
    path: '/showcase/3dgs/m_1.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 1, -1],
    rotation: [0, 0, 0, 1],
    scale: [0.18, 0.18, 0.18]
  },

]);

const splatFiles = ref([
  {
    path: './showcase/3dgs/m_3_new.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 0, 0],
    rotation: [1, 0, 0, 0],
    scale: [1.0, 1.0, 1.0]
  },
  {
    path:'./showcase/3dgs/m_1_new.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 0, 0],
    rotation: [1, 0, 0, 0],
    scale: [1.0, 1.0, 1.0],
  },
  {
    path: './showcase/3dgs/m_2_new.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 0, 0],
    rotation: [1, 0, 0, 0],
    scale: [1.0, 1.0, 1.0],
  },
  {
    path: './showcase/3dgs/m_13.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 0, 0],
    rotation: [1, 0, 0, 0],
    scale: [1.0, 1.0, 1.0],
  },
  {
    path: './showcase/3dgs/m_8.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 0, 0],
    rotation: [1, 0, 0, 0],
    scale: [1.0, 1.0, 1.0],
  },
  {
    path: './showcase/3dgs/m_11_new.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 0, 0],
    rotation: [1, 0, 0, 0],
    scale: [1.0, 1.0, 1.0],
  },
  {
    path: '/showcase/3dgs/f_14_arm_down.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 0, 0],
    rotation: [1, 0, 0, 0],
    scale: [1.0, 1.0, 1.0],
  },
  {
    path: '/showcase/3dgs/f_0_arm_down.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 0, 0],
    rotation: [1, 0, 0, 0],
    scale: [1.0, 1.0, 1.0],
  },
  {
    path: '/showcase/3dgs/f_1_arm_down.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 0, 0],
    rotation: [1, 0, 0, 0],
    scale: [1.0, 1.0, 1.0],
  },
  {
    path: '/showcase/3dgs/f_2_arm_down.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 0, 0],
    rotation: [1, 0, 0, 0],
    scale: [1.0, 1.0, 1.0],
  },
  {
    path: '/showcase/3dgs/f_9_arm_down.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 0, 0],
    rotation: [1, 0, 0, 0],
    scale: [1.0, 1.0, 1.0],
  },
  {
    path: '/showcase/3dgs/f_5_arm_down.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 0, 0],
    rotation: [1, 0, 0, 0],
    scale: [1.0, 1.0, 1.0],
  },


]);

const splatFilesInViewers = ref(new Array(showCases.value.length).fill(null));



onMounted(() => {

  showCases.value.forEach((item, index) => {
    hasInitialized.value[index] = false;
    const gs = gsContainers.value[index];
    if (gs) {
      // 创建一个遮罩层
      const overlay = document.createElement('div');
      overlay.className = 'overlay';
      overlay.innerText = '放置加载 3D 视图';
      overlay.style.borderRadius = '10px';
      gs.style.position = 'relative';
      gs.appendChild(overlay);

      const viewer = new GaussianSplats3D.Viewer({
        rootElement: gs,
        cameraUp: [0, -1, 0],
        initialCameraPosition: [-0, -0.4, -1.2], // a little bit down
        initialCameraLookAt: [0., -0.38, -1.],
        // initialCameraPosition: [-0, -0, -1.5],
        // initialCameraLookAt: [0., 0., -1.],
        sharedMemoryForWorkers: false,
        dynamicScene: false
      });

      gs.addEventListener('mouseenter', () => {
        overlay.style.display = 'none'; // 隐藏图标
      });
      //
      gs.addEventListener('mousemove', () => {
        overlay.style.display = 'none';
      });

      gs.addEventListener('mouseleave', () => {
        overlay.style.display = 'flex';
        overlay.style.border = ''; // 添加 2px 红色半透明边框
        overlay.style.borderRadius = ''; // 圆角
      });

      // watch splatFilesInViewers[index]是否发生变化，如果不为null

      watch(
          () => splatFilesInViewers.value[index],
          (newItem, oldItem) => {

            overlay.innerText = '';

            if (newItem) {
              // overlay.style.display = 'none';
              console.log('viewer', index, 'changed from', oldItem, 'to', newItem);
              if (oldItem) {
                viewer.addSplatScene(newItem.path, {
                  splatAlphaRemovalThreshold: newItem.splatAlphaRemovalThreshold,
                  showLoadingUI: newItem.showLoadingUI,
                  position: newItem.position,
                  rotation: newItem.rotation,
                  scale: newItem.scale,
                })
                    .then(() => {
                      viewer.removeSplatScene(0)
                          .then(() => {
                            viewer.start();
                            viewer.perspectiveControls.stopListenToKeyEvents();
                            viewer.orthographicControls.stopListenToKeyEvents();
                          })
                    })
              }
              else {
                viewer
                    .addSplatScene(newItem.path, {
                      splatAlphaRemovalThreshold: newItem.splatAlphaRemovalThreshold,
                      showLoadingUI: newItem.showLoadingUI,
                      position: newItem.position,
                      rotation: newItem.rotation,
                      scale: newItem.scale,
                    })
                    .then(() => {
                      viewer.start();
                      viewer.perspectiveControls.stopListenToKeyEvents();
                      viewer.orthographicControls.stopListenToKeyEvents();
                    });
              }


            }
          },
          { deep: true }

      );

      viewers.value[index] = viewer;

      // gs.removeChild(overlay);
      // overlay.style.display = 'none';

    }
  });
});

onBeforeUnmount(() => {

  viewers.value.forEach((viewer) => {
    viewer.dispose();
  });

});


</script>

<template>
  <div>
<!--    <el-divider />-->

<!--    <el-row justify="center">-->
<!--      <h1 class="section-title"> Showcase </h1>-->
<!--    </el-row>-->
    <el-row justify="center">
      <p style="color: gray;"> Try to drag and drop a character to the viewer for detail observation! </p>
    </el-row>

    <el-row justify="center" :gutter="10">
      <el-col
          v-for="(item, index) in showCases"
          :key="index"
          :xs="10" :sm="10"
          :md="4"  :lg="4"  :xl="4"
          class="stage-container"
      >

        <div
            ref="gsContainers" class="gs-container"
            @dragover="handleDragOver"
            @dragleave="handleDragLeave"
            @drop="handleDrop"
        >
          <div
              ref="addIconRefs"
              class="add-icon"
              v-show="showAddIcon"
          ></div>
        </div>
        <!--          <div id="gs" class="gs-container"></div>-->

        <img src="/showcase/stage/stage.png" class="stage" />
        <p align="center" style="font-family: 'Courier New', cursive, sans-serif;">
          STAGE {{ index }}
        </p>
      </el-col>
    </el-row>
  </div>
</template>


<style>

.flashing {
  animation: flash-scale 0.5s infinite alternate;
}

@keyframes flash {
  0% { opacity: 1; }
  100% { opacity: 0.5; }
}

@keyframes flash-scale {
  0% { opacity: 1; transform: translate(-50%, -50%) scale(1); }
  100% { opacity: 0.5; transform: translate(-50%, -50%) scale(1.2); }
}


.stage-container {
  position: relative; /* 让子元素的 absolute 定位相对于此元素 */
  text-align: center;
}

.stage {
  position: absolute; /* 绝对定位 */
  top: 80%; /* 图片垂直居中 */
  left: 50%; /* 图片水平居中 */
  transform: translate(-50%, -50%); /* 调整位置，使其居中 */
  width: 75%; /* 让图片大小适应 */
  height: 25%;
  z-index: -1; /* 让图片在底层 */
  object-fit: fill; /* 强制变形 */
}

/* 鼠标拖动时变灰 */
.gs-container:active .add-icon {
  background: rgba(128, 128, 128, 0.5); /* 拖动时变灰色 */
}

.gs-container {
  width: 100%;
  aspect-ratio: 3 / 4;
  margin: 6px;
  position: relative; /* 允许子元素绝对定位 */
}

.add-icon {
  position: absolute;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background: rgba(0, 255, 0, 0.3); /* 默认绿色 */
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  font-weight: bold;
  color: white;
  top: 50%;
  left: 50%;
  transition: background 0.3s ease-in-out;
  transform: translate(-50%, -50%); /* 精准居中 */
  transform-origin: center;
}

/* 添加 "+" 符号 */
.add-icon::after {
  content: "+";
  font-size: 30px;
  font-weight: bold;
  color: white;
}



.spinnerPrimary0 {
  display: none !important;
}

.spinnerOuterContainer0 {
  height: 100% !important;
  margin: 0 auto !important;
  top: 0 !important;
  left: 0 !important;
}

.spinnerContainerPrimary0 {
  padding-top: 0% !important;
  position: relative !important;
  transform: none !important;
  width: fit-content !important;
  margin: 0 auto !important;
  left: 0 !important;
  padding: 10px 20px !important;
}

.messageContainerPrimary0 {
  padding-top: 0% !important;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.0);
  display: flex;
  align-items: center;
  justify-content: center;
  color: #757575;
  cursor: pointer;
  font-size: 16px;
  font-weight: bold;
  text-align: center;
}


</style>
