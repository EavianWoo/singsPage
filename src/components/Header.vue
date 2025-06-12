<script lang="ts" setup>

import { ElIcon } from 'element-plus'
import { HomeFilled, SuccessFilled } from '@element-plus/icons-vue'

// 个人主页
const home = {
    name: "My Blog",
    link: "",
}

// 项目主页
const logo = {
    name: "Template",
    link: "#",
}

const dynamic_logo = './dynamic_logo.gif'

// 右侧更多菜单
const more = [
    {
        name: "Not Implemented now",
        link: "xx",
    },
    {
        name: "yy",
        link: "yy",
    },
]

import { ref, onMounted } from 'vue'

const headerStyle = ref({})

onMounted(() => {
  window.addEventListener('scroll', () => {
    if (window.scrollY > 50) {
      headerStyle.value = {
        backgroundColor: 'transparent',  // 这里改为透明背景
        boxShadow: '0px 4px 6px rgba(0, 0, 0, 0.1)',  // 可根据需求保留阴影
      }
    } else {
      headerStyle.value = {
        backgroundColor: 'transparent',
        boxShadow: 'none',
      }
    }
  })
})

</script>

<template>
    <!-- 设置页首始终悬浮顶部 -->
    <el-affix :offset="0" :style="{width: '100%'}">
    
    <!-- 水平导航目录 -->
    <el-menu 
        default-active="1"
        class="el-menu-demo"
        mode="horizontal"
        :ellipsis="false"
        :style="headerStyle"
    >
        <!-- 个人主页 -->
        <el-menu-item index="0"  style="margin-left:25px">
<!--            <el-icon :size="20"><HomeFilled /></el-icon>-->
            <el-icon :size="20">
              <a href="https://github.com/EavianWoo/SinGS" target="_blank" rel="noopener noreferrer">
                <img
                    :src="dynamic_logo"
                    alt="Site Logo"
                    class="logo"
                />
              </a>
            </el-icon>
        </el-menu-item>
        
<!--        &lt;!&ndash; 项目主页 &ndash;&gt;-->
<!--        <el-menu-item index="1">-->
<!--            <el-icon :size="20"><SuccessFilled /></el-icon>-->
<!--            <a :href=logo.link>{{ logo.name }} </a>-->
<!--        </el-menu-item>-->

        <!-- 更多栏目 -->
        <el-sub-menu index="2" style="margin-left:auto">
            <template #title>More</template>
            <el-menu-item index="2-1">
                <a :href=more[0].link>{{ more[0].name }}</a>
            </el-menu-item>
<!--            <el-menu-item index="2-2">-->
<!--                <a :href=more[1].link>{{ more[1].name }}</a>-->
<!--            </el-menu-item>-->
        </el-sub-menu>

    </el-menu>
    </el-affix>
</template>

<style>

/* 导航背景 */
.el-menu {
	background: none;
}

/* 下拉菜单宽度 */
.el-menu--popup {
	min-width: 100%;
}

/* 导航背景特效和阴影 */
.el-affix--fixed {
	box-shadow: 1px 1px 2px rgba(0, 0, 0, 0.1);
	background-image: radial-gradient(#ffffffca 1px,#ffffff 1px);
	background-size: 3px 3px;
}

/* 水平导航高度 */
.el-header {
    --el-header-height: 50px;
}

</style>

<style scoped>

/* 水平导航左右分区 */
.el-menu--horizontal > .el-menu-item:nth-child(2) {
    margin-right: auto;
}

/* 水平导航高度 */
.el-menu--horizontal {
    --el-menu-horizontal-height: 60px;
}

.logo {
  height: 50px;
  transition: opacity 0.3s;
  margin-left: -25px;

  &:hover {
    opacity: 0.8;
  }
}

/* 取消鼠标焦点悬浮在链接上的颜色装饰 */
a:hover {
  color: inherit;
  border-bottom: none;
}

/* 链接装饰，取消下划线和链接颜色 */
a {
	text-decoration: None;
	color: inherit;
}

</style>

  