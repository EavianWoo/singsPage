<script setup>
import abstract from '../mds/framework.mdx';
import { ref, onMounted } from 'vue';
import PdfViewer from 'vue3-pdfjs';

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

    <el-row justify="center">
      <el-col :xs="24" :sm="20" :md="16" :lg="16" :xl="16">
        <div class="pdf-wrapper">
          <embed src="/framework/framework.pdf#view=FitH"
                 type="application/pdf"
                 class="pdf-embed">
        </div>
      </el-col>
    </el-row>

    <el-row justify="center">
      <p> Fig. Framework </p>
    </el-row>

  </div>
</template>

<style scoped>
.markdown-container {
  text-align: justify;
  line-height: 1;
  font-size: 10px;
}

/* 针对引用的 caption 控制灰色字体 */
.markdown-container blockquote,
.markdown-container blockquote p {
  font-size: 12px; /* 设置灰色 caption 字体大小 */
  color: #c63838;     /* 设置灰色 */
  font-style: italic; /* 可选：设置斜体 */
}

/* 使用 ::v-deep 确保样式被应用到子组件渲染的元素 */
.markdown-container ::v-deep p {
  font-size: 14px;
  line-height: 1.5;
  color: gray; /* 灰色正文文字 */
}

.pdf-wrapper {
  width: 100%;
  aspect-ratio: 4.73;  /* FIXED */
  background: white;
  display: flex;
  justify-content: center;
  align-items: center;
}

.pdf-embed {
  width: 100%;
  height: 250%;
  object-fit: contain; /* 让 PDF 适应容器，同时保持原始比例 */
}
</style>