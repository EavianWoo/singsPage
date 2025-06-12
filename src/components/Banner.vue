<template>
  <header :class="['banner', { 'scrolled': isScrolled }]">
    <div class="banner-content">
      <!-- 左侧 Logo -->
      <router-link to="/" class="logo-link">
        <img
            :src="dynamic_logo"
            alt="Site Logo"
            class="logo"
        />
      </router-link>

      <!-- 右侧导航 -->
      <nav class="navigation">
        <ul class="nav-list">
          <li
              v-for="(item, index) in navItems"
              :key="index"
              class="nav-item"
          >
            <a
                :href="item.path"
                class="nav-link"
                :class="{ 'active': activeNav === item.path }"
                @click.prevent="handleNavClick(item.path)"
            >
              <span class="link-text">{{ item.text }}</span>
              <div class="link-underline"></div>
            </a>
          </li>
        </ul>
      </nav>
    </div>
  </header>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { useRoute } from 'vue-router'

const route = useRoute()
const isScrolled = ref(false)
const activeNav = ref('/')

const dynamic_logo = './dynamic_logo.gif'

const navItems = [
  { text: 'Home', path: '/' },
  { text: 'Projects', path: '/projects' },
  { text: 'About', path: '/about' },
  { text: 'Contact', path: '/contact' }
]

// 处理滚动事件
const handleScroll = () => {
  isScrolled.value = window.scrollY > 50
}

// 导航点击处理
const handleNavClick = (path) => {
  activeNav.value = path
  // 这里可以添加路由跳转逻辑
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

// 生命周期钩子
onMounted(() => {
  window.addEventListener('scroll', handleScroll)
  activeNav.value = route.path
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})
</script>

<style lang="scss" scoped>
.banner {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  width: 100%;
  z-index: 1000;
  background: rgba(255, 255, 255, 0);  /* 初始透明 */
  backdrop-filter: blur(0px);
  transition: all 0.3s ease;
  height: 60px;  /* 设置固定高度 */


  &.scrolled {
    background: rgba(255, 255, 255, 0.95);
    backdrop-filter: blur(10px);
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }

  .banner-content {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0rem 0rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .logo-link {
    .logo {
      height: 60px;
      transition: opacity 0.3s;
      margin-left: -25px;

      &:hover {
        opacity: 0.8;
      }
    }
  }

  .navigation {
    .nav-list {
      display: flex;
      gap: 2rem;
      list-style: none;
      margin: 0;
      padding: 0;
    }

    .nav-item {
      position: relative;
    }

    .nav-link {
      color: var(--color-text);
      text-decoration: none;
      font-weight: 500;
      font-size: 0.95rem;
      position: relative;
      padding: 0.5rem 0;
      transition: color 0.3s;

      &.active {
        color: var(--color-primary);

        .link-underline {
          width: 100%;
          opacity: 1;
        }
      }

      &:hover {
        color: var(--color-primary);

        .link-underline {
          width: 100%;
          opacity: 1;
        }
      }

      .link-underline {
        position: absolute;
        bottom: 0;
        left: 0;
        width: 0;
        height: 2px;
        background: var(--color-primary);
        opacity: 0;
        transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
      }
    }
  }
}

@media (max-width: 768px) {
  .banner {
    .banner-content {
      padding: 1rem;
    }

    .navigation {
      .nav-list {
        gap: 1.5rem;
      }

      .nav-link {
        font-size: 0.9rem;
      }
    }
  }
}
</style>