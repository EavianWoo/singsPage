<script lang="ts" setup>
// 页脚需要的链接
const links = {
    Nerfies: "https://github.com/nerfies/nerfies.github.io",
    Vue: "https://cn.vuejs.org/",
    Vite: "https://cn.vitejs.dev/",
    ElementPlus: "https://element-plus.org/zh-CN/",
    template: "https://github.com/JunyaoHu/academic-project-page-template-vue",
}
</script>

<template>
  
  <div class="bg">
    
<!--    <el-watermark :content="['SinGS']" :gap="['60','0']" :z-index="0">-->
    <el-watermark
        image="./icon/FullLogo_200x300.png"
        :width="30"
        :height="20"
    >
      <el-row justify="center">
        <p class="footer">
          This page is inspired by
          <a :href="links.template"> this project </a>
          , which integrates many useful components.
          <br/>
          Parts of this project were adopted directly from it, and here is my sincere appreciation to the author and the contribution.
          <br/>

        </p>
      </el-row>
    </el-watermark>
  </div>
</template>

<style scoped>

/* 页脚背景 */
.bg {
  background-color: rgb(255, 255, 255);
}

/* 页脚文字 */
.footer {
  text-align: center;
  margin: 20px;
  z-index: 1;
}

</style>