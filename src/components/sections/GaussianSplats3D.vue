<script setup lang="ts">
import { onMounted, onBeforeUnmount, ref } from 'vue';
import * as GaussianSplats3D from '@mkkellogg/gaussian-splats-3d';

const viewers = ref<GaussianSplats3D.Viewer[]>([]);
const gsContainers = ref<(HTMLElement | null)[]>([]);
const hasInitialized = ref<boolean[]>([]); // 记录哪些 viewer 已经初始化

// import * as THREE from 'three';

// define .splat files
// const splatFrames = [
//   '/3dgs/splatFrames/anim_00000.splat',
//   '/3dgs/splatFrames/anim_00001.splat',
//   // '/3dgs/splatFrames/anim_00002.splat',
//   // '/3dgs/splatFrames/anim_00003.splat',
//   // '/3dgs/splatFrames/anim_00004.splat',

//
// ]
let angle = 0;
const radius = 4; // 旋转半径
const speed = 0.02; // 旋转速度
let animationFrameId: number | null = null;



const viewerOpts = [

]


const showCases = [
  {
    path: './3dgs/cotton-mirror.splat',
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
    position: [0, 0, 0],
    rotation: [1, 0, 0, 0],
    scale: [0.5, 0.5, 0.5],
    visible: true,
  },
  {
    path: '/3dgs/converted.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: false,
    position: [0, 0, -0.5],
    rotation: [1, 0, 0, 0],
    scale: [0.5, 0.5, 0.5],
    visible: false,
  },
  {
    path: './3dgs/cotton-mirror.splat',
    splatAlphaRemovalThreshold: 5,
    showLoadingUI: true,
    position: [0, 1, -1],
    rotation: [0, 0, 0, 1],
    scale: [0.18, 0.18, 0.18]
  },
  // {
  //   path: '/3dgs/converted.splat',
  //   splatAlphaRemovalThreshold: 5,
  //   showLoadingUI: false,
  //   position: [0, 0, -1],
  //   rotation: [1, 0, 0, 0],
  //   scale: [0.5, 0.5, 0.5],
  //   visible: false,
  // },
  // {
  //   path: '/3dgs/converted.splat',
  //   splatAlphaRemovalThreshold: 5,
  //   showLoadingUI: false,
  //   position: [0, 0, -1.5],
  //   rotation: [1, 0, 0, 0],
  //   scale: [0.5, 0.5, 0.5],
  //   visible: false,
  // },

]

onMounted(() => {

    showCases.forEach((item, index) => {
      hasInitialized.value[index] = false;
      const gs = gsContainers.value[index];
      if (gs) {
        // 创建一个遮罩层
        const overlay = document.createElement('div');
        overlay.className = 'overlay';
        overlay.innerText = '点击加载 3D 视图';
        gs.style.position = 'relative';
        gs.appendChild(overlay);

        overlay.addEventListener('click', () => {
          if (hasInitialized.value[index]) return;
          hasInitialized.value[index] = true;

          const viewer = new GaussianSplats3D.Viewer({
            rootElement: gs,
            cameraUp: [0, -1, 0],
            initialCameraPosition: [-2.13, -0.50, -4.63],
            initialCameraLookAt: [0., 0., -1.],
            sharedMemoryForWorkers: false,
            dynamicScene: true
          });

          viewer
              .addSplatScene(item.path, {
                splatAlphaRemovalThreshold: item.splatAlphaRemovalThreshold,
                showLoadingUI: item.showLoadingUI,
                position: item.position,
                rotation: item.rotation,
                scale: item.scale,
              })
              .then(() => {
                viewer.start();
                viewer.perspectiveControls.stopListenToKeyEvents();
                viewer.orthographicControls.stopListenToKeyEvents();
              });

          viewers.value.push(viewer);

          gs.removeChild(overlay);
        });
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
      <el-divider />

      <el-row justify="center">
        <h1 class="section-title"> Showcase </h1>
      </el-row>
  
      <el-row justify="center" :gutter="10">
        <el-col
          v-for="(item, index) in showCases"
          :key="index"
          :xs="10" :sm="10"
          :md="4"
          :lg="4"
          :xl="4"
        >

<!--          <el-col :xs="16" :sm="21" style="margin: 0px auto;">-->
          <div ref="gsContainers" class="gs-container"></div>
<!--          <div id="gs" class="gs-container"></div>-->
<!--          </el-col>-->
          <p>
            Showcase X
          </p>
        </el-col>
      </el-row>
    </div>
  </template>


<style>

.gs-container {
  width: 100%;
  aspect-ratio: 3 / 4;
  margin: 5px;

}

//border: 2px solid #000;
//border-radius: 8px;
//padding: 10px;
//box-sizing: border-box;

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
