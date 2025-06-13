<script lang="ts" setup>

import { ElIcon } from 'element-plus'
import { DataAnalysis} from '@element-plus/icons-vue'
import { Github, Youtube, Wand} from "lucide-vue-next";
import ArxivIcon from '@/assets/icons/arXiv-3.svg'

// gif logo
import { defineProps } from 'vue';
import { defineComponent, computed } from 'vue';

const props = defineProps({
  gifSrc: {
    type: String,
    required: true
  },
  altText: {
    type: String,
    default: 'Logo'
  },
});

// 定义一个静态的 news 字符串
const news = "🔥 [ DATE ]  Peace and Love ~ No news.";

// 计算当前日期并替换 [DATE]
const formattedNews = computed(() => {
  const currentDate = new Date();
  const formattedDate = currentDate.toISOString().split('T')[0]; // 获取格式化的日期 (YYYY-MM-DD)
  return news.replace('DATE', formattedDate);
});


// logo地址，没有则置为""即可
const static_logo = './ICCI_logo.png'
const dynamic_logo = './dynamic_logo.gif'

// 标题
const title_seq_0 = 'Sin'
const title_seq_1 = 'G'
const title_seq_2 = 'S'
// const title_seq_left = ': Animatable Single-Image Human Gaussian Splats with Kinematic Priors'

const title_seq_left = [': Animatable', ' Sin', 'gle-Image Human ',  ' G', 'aussian', ' S', 'plats with Kinematic Priors']

// 标题颜色
const title_seq_0_color = '#2b8cd4'
const title_seq_1_color = '#ff5758'
const title_seq_2_color = '#7fd858'
// const title_seq_left_color = '#323232'
const title_seq_left_color = ['#323232', '#2b8cd4', '#323232', '#ff5758', '#323232', '#7fd858', '#323232']

// 标题补充，没有则置为''即可
const title_supp = ''

// 标题补充颜色
const title_supp_color = '#42B883'

// 按钮颜色
const btn_color = '#444444'

// 作者清单（包含作者姓名、头像、主页、地址序号）
const authors = [
  {
    name: "Yufan Wu",
    icon: "./icon/authors/mine.jpeg",
    homepage: "https://github.com/EavianWoo/",
    address_flag: "1, 2"
  },
  {
    name: "Xuanhong Chen",
    icon: "./icon/authors/xuanhongChen.png",
    homepage: "https://github.com/neuralchen",
    address_flag: "1, 2, *"
  },
  {
    name: "Wen Li",
    icon: "./icon/authors/wenLi.gif",
    homepage: "https://github.com/lililuya",
    address_flag: "1, 2"
  },
  {
    name: "Shunran Jia",
    icon: "./icon/authors/unkown/unknown.jpeg",
    // homepage: "",
    address_flag: ""
  },
  {
    name: "Hualiang Wei",
    icon: "./icon/authors/unkown/unknown.jpeg",
    // homepage: "",
    address_flag: "1, 2"
  },
  {
    name: "Kairui Feng",
    icon: "./icon/authors/unkown/unknown.jpeg",
    // homepage: "",
    address_flag: "1"
  },
  {
    name: "Jialiang CHEN",
    icon: "./icon/authors/unkown/unknown.jpeg",
    // homepage: "",
    address_flag: ""
  },
  {
    name: "Yuhan Li",
    icon: "./icon/authors/unkown/unknown.jpeg",
    // homepage: "",
    address_flag: "1"
  },
  {
    name: "Ang He",
    icon: "./icon/authors/unkown/unknown.jpeg",
    // homepage: "",
    address_flag: "1"
  },
  {
    name: "Weimin Zhang",
    icon: "./icon/authors/unkown/unknown.jpeg",
    // homepage: "",
    address_flag: "1"
  },
  {
    name: "Bingbing Ni",
    icon: "./icon/authors/unkown/unknown.jpeg",
    // homepage: "",
    address_flag: "1"
  },
  {
    name: "Wenjun Zhang",
    icon: "./icon/authors/unkown/unknown.jpeg",
    // homepage: "",
    address_flag: "1"
  },
]

// 地址清单（包含地址名称、头像、主页、地址序号）
const addresses = [
  {
    address_flag: "1",
    name: "Shanghai Jiao Tong University",
    icon: "./icon/SJTU_logo.png",
    homepage: "https://en.sjtu.edu.cn/"
  },
  {
    address_flag: "2",
    name: "ICCI, USC-SJTU",
    icon: "./icon/ICCI.jpeg",
    homepage: "https://icci.sjtu.edu.cn/"
  },
  // {
  //   address_flag: "",
  //   name: "Akool",
  //   icon: "./icon/Akool_logo.png",
  //   homepage: "https://www.akool.com/"
  // },
  // {
  //   address_flag: "",
  //   name: "SocialBook",
  //   icon: "./icon/SocialBook_logo.png",
  //   homepage: "https://socialbook.io/"
  // },
]
const SpecialAck = [
  {
    address_flag: "",
    name: "Akool",
    icon: "./icon/Akool_logo.png",
    homepage: "https://www.akool.com/"
  },
  {
    address_flag: "",
    name: "SocialBook",
    icon: "./icon/SocialBook_logo.png",
    homepage: "https://socialbook.io/"
  },
]

// 共一和通讯提示
const con_and_corresponding_author = 
  "*: Corresponding Author."


// 强调内容
const emphases = [
  "🎉 [CVPR 2025]",
]


// 提供引导资料链接
const buttons = [
  {
    disabled: false,
    name: "Paper",
    link: "https://openaccess.thecvf.com/content/CVPR2025/papers/Wu_SinGS_Animatable_Single-Image_Human_Gaussian_Splats_with_Kinematic_Priors_CVPR_2025_paper.pdf",
    component: ArxivIcon,
  },
  {
    disabled: false,
    name: "Code",
    link: "https://github.com/EavianWoo/SinGS",
    component: Github,
  },
  {
    disabled: true,
    name: "Demo",
    link: "https://eavianwoo.github.io/singsPage/",
    component: Wand,
  },
  {
    disabled: true,
    name: "Slide",
    component: DataAnalysis,
  },
  {
    disabled: false,
    name: "Video",
    link: "https://youtu.be/2NVqoVNVmjY",
    component: Youtube,
  },
]

</script>

<template>
  <div>

    <!-- 最新消息提示 -->
    <el-row justify="center">
      <el-col :span="24">
        <el-alert :title="formattedNews" type="success" />
      </el-col>
    </el-row>

    <!-- 文章logo -->
<!--    <el-row v-if="dynamic_logo" justify="center">-->
<!--&lt;!&ndash;      <el-image :src="dynamic_logo" class="logo" fit="cover" />&ndash;&gt;-->
<!--      <el-image :src="dynamic_logo" alt="altText" class="gif-container"/>-->

<!--    </el-row>-->

    <!-- 文章标题 -->
    <el-row justify="center">
      <el-col :span="20">
        <h1 class="paper-title">
          <span v-if="title_seq_0" :style="{color:title_seq_0_color}"> {{ title_seq_0 }}</span>
          <span v-if="title_seq_1" :style="{color:title_seq_1_color}"> {{ title_seq_1 }}</span>
          <span v-if="title_seq_2" :style="{color:title_seq_2_color}"> {{ title_seq_2 }}</span>
<!--          <span v-if="title_seq_left" :style="{color:title_seq_left_color}"> {{ title_seq_left }}</span>-->
          <span v-for="(word, index) in title_seq_left" :key="index"
                :style="{color: title_seq_left_color[index], lineHeight: '1.5' } ">
            {{ word }}</span>
          <span v-if="title_supp" :style="{color:title_supp_color}"> {{ title_supp }}</span>
        </h1>
      </el-col>
    </el-row>

    <!-- 作者名单 -->
    <el-row justify="center">
      <el-col :xs="20" :sm="18" :md="16" :lg="16" :xl="16" >
        <div class="authors-container">
          <a :href=author.homepage v-for="author in authors">
            <el-button class="title-button" type="primary" text>
              <el-avatar v-if="author.icon" :size="40" :src="author.icon" />
              <span class="author">
                {{ author.name }}<sup v-if="author.address_flag" class="name_sup">{{ author.address_flag }}</sup>
              </span>
            </el-button>
          </a>
        </div>
      </el-col>
    </el-row>

    <!-- 地址名单 -->
    <el-row justify="center">
      <a :href=address.homepage v-for="address in addresses">
        <el-button class="title-button" type="primary" text>
          <el-avatar v-if="address.icon" :size="40" :src="address.icon" />
          <span class="address">
            <sup v-if="address.address_flag" class="address_sup">{{ address.address_flag }}</sup>{{ address.name }}
          </span>
        </el-button>
      </a>
    </el-row>

    <!-- 共一和通讯提示内容 -->
    <el-row justify="center" class="con-cor">
        {{ con_and_corresponding_author }}
    </el-row>

    <!-- 强调内容 -->
    <el-row justify="center" class="emphasis" v-for="emphasis in emphases">
        {{ emphasis }}
    </el-row>

    <!-- 提供引导按钮 -->
    <el-row justify="center" style="margin-bottom: 20px;">
      <el-col :span="20">
        <el-row justify="center">
          <a :href=button.link v-for="button in buttons">
            <el-button class="guidance-button" size="default" :color="btn_color" :disabled="button.disabled" round>
              <el-icon :size="20">
                <component :is="button.component" />
              </el-icon>
              <span class="btn-text">{{ button.name }}</span>
            </el-button>
          </a>
        </el-row>
      </el-col>
    </el-row>

    <!-- special acknowledgement -->
    <el-row  justify="center">
      <p style="
          font-family: 'Georgia', serif;
          font-size: 20px;
          font-weight: bold;
          color: #333;
          text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
          letter-spacing: 1px;
          margin: 20px 0;
          "> Special Acknowledgement </p>
    </el-row>
    <el-row justify="center">

      <a :href=address.homepage v-for="address in SpecialAck">
        <el-button class="title-button" type="primary" text>
          <el-avatar v-if="address.icon" :size="40" :src="address.icon" />
          <span class="address">
            <sup v-if="address.address_flag" class="address_sup">{{ address.address_flag }}</sup>{{ address.name }}
          </span>
        </el-button>
      </a>
    </el-row>
  </div>
</template>

<style scoped>

/* 文章标题字体、字间距、居中排布、字号 */
.paper-title {
  font-family: "MyFont", Verdana, sans-serif;
  letter-spacing: 2px;
  font-size: 42px;
  margin: 32px;
  text-align: center;
}

/* 姓名和地址按钮 */
.title-button {
  margin: 10px 3px;
}

/* 姓名和地址按钮光标悬浮 */
.title-button:hover {
  margin: 10px 8px;
}

/* 引导材料按钮 */
.guidance-button {
  margin: 8px 5px;
  box-shadow: #d8d8d8 1px 1px 1px 1px;
}

.authors-container {
  display: flex;
  justify-content: center; /* 居中 */
  flex-wrap: wrap; /* 自动换行 */
  gap: 10px; /* 设置间距 */
}

/* 姓名属性 */
.author {
  font-size: 18px;
  margin-left: 3px;
  color: #212121
}


/* 姓名上标属性 */
.name_sup {
  color: #606266; 
  margin-left: 3px;
}

/* 地址属性 */
.address {
  font-size: 18px;
}

/* 地址上标属性 */
.address_sup {
  color: #606266; 
  margin-right: 1px;
}

/* 头像属性 */
.el-avatar {
  margin-right: 6px;
  box-shadow: #b7b7b7 0px 0px 3px 1px;
}

/* 共一和通讯文字属性 */
.con-cor {
  font-family: Arial;
  font-size: 12px;
  margin: 18px 0px;
  text-align: center;
}

/* 强调信息属性 */
.emphasis {
  color: chocolate;
  font-weight: bold;
  margin: 8px;
  font-size: 22px;
  text-align: center;
}

/* 引导材料按钮文字属性 */
.btn-text {
  font-size: 18px;
  color: #ffffff;
}

.el-alert {
  margin: 10px 0 0;
}

.el-alert:first-child {
  margin: 0;
}

.logo {
  width: 150px; 
  height: 150px;
  border-radius: 50%;
  box-shadow: #ced3dc 0px 0px 3px 2px;
  margin-top: 40px;
}

.gif-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 200px;
  height: auto;
}

.gif-logo {
  width: 50%;
  height: auto;
}

/* 手机端链接样式处理 */
a:-webkit-any-link {
  text-decoration: none;
}


&::before {
  content: '';
  position: absolute;
  inset: -2px;
  background: linear-gradient(45deg, #ff6b6b, #4ecdc4);
  z-index: -1;
  border-radius: 14px;
  filter: blur(8px);
  opacity: 0.5;
  transition: opacity 0.3s;
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