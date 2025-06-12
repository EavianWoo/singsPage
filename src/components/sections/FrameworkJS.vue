<template>
  <div class="pdf-wrapper" style="width: 100%; height: 600px; overflow: auto; border: 1px solid #ccc;">
    <canvas ref="pdfCanvas"></canvas>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';

const pdfCanvas = ref(null);

onMounted(async () => {
  // 动态加载 PDF.js（也可以直接通过 script 标签引入）
  const pdfjsLib = await import('pdfjs-dist/build/pdf');

  // 设置worker路径
  pdfjsLib.GlobalWorkerOptions.workerSrc =
      'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.14.305/pdf.worker.min.js';

  const url = './framework/framework.pdf'; // 你的 PDF 路径

  const loadingTask = pdfjsLib.getDocument(url);

  loadingTask.promise.then(pdfDoc => {
    // 默认显示第一页
    pdfDoc.getPage(1).then(page => {
      const scale = 1.5;
      const viewport = page.getViewport({ scale });
      const canvas = pdfCanvas.value;
      const context = canvas.getContext('2d');

      canvas.height = viewport.height;
      canvas.width = viewport.width;

      const renderContext = {
        canvasContext: context,
        viewport
      };
      page.render(renderContext);
    });
  }).catch(err => {
    console.error('Error loading PDF:', err);
  });
});
</script>

<style scoped>
.pdf-wrapper {
  max-width: 800px;
  margin: 0 auto;
}
canvas {
  display: block;
  margin: 0 auto;
}
</style>
