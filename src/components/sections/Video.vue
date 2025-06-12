<script setup>
</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">Video</h1>
    </el-row>

    <!-- 每个网站的视频的iframe可能不一致，最好在这里手动调整 -->
    <el-row justify="center">
      <el-col :xs="20" :sm="18" :md="16" :lg="16" :xl="16">

        <!-- local -->
        <el-container class="video-container">
          <video controls muted preload playsinline>
            <source src="/video/anim_test.mp4" type="video/mp4">
          </video>
        </el-container>

      </el-col>
    </el-row>
  </div>
</template>

<style scoped>

.video-container{
  margin: 20px 0px 0px 0px;
}

iframe, video {
  aspect-ratio: 16 / 9;
  width: 100%;
}

</style>