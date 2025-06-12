<script setup>
import abstract from '../mds/abstract.mdx';
</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
      <el-col :xs="24" :sm="20" :md="16" :lg="16" :xl="16">
        <div class="markdown-container">
          <abstract />
        </div>
      </el-col>
    </el-row>

  </div>
</template>

<style scoped>
.markdown-container {
  text-align: justify;
  line-height: 2;
}
</style>