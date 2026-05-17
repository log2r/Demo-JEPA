<script lang="ts" setup>
import { ElIcon } from 'element-plus'
import { Document, Files, MagicStick, Picture, DataAnalysis, Film } from '@element-plus/icons-vue'

// logo地址，没有则置为""即可
const logo = './logo.png'

// 标题
const title = 'Demo-JEPA: Joint-Embedding Predictive Architecture for One-shot Cross-Embodiment Imitation'

// 标题颜色
const title_color = '#000000'

// 标题补充，没有则置为''即可
const title_supp = ''

// 标题补充颜色
const title_supp_color = '#42B883'

// 按钮颜色
const btn_color = '#444444'

// 作者清单（包含作者姓名、头像、主页、地址序号）
const authors = [
  {
    name: "Jingyang He",
    icon: "",
    homepage: "https://github.com/banban3forever",
    address_flag: "1,3,*"
  },
  {
    name: "Guangrun Li",
    icon: "",
    homepage: "https://github.com/log2r",
    address_flag: "1,3,*"
  },
  {
    name: "Jieyu Zhang",
    icon: "",
    homepage: "https://jieyuz2.github.io/",
    address_flag: "2,*,†"
  },
  {
    name: "Chengkai Hou",
    icon: "",
    homepage: "https://jackhck.github.io/",
    address_flag: "1,3"
  },
  {
    name: "Zhengping Che",
    icon: "",
    homepage: "https://chezhengping.xyz/",
    address_flag: "3"
  },
  {
    name: "Shanghang Zhang",
    icon: "",
    homepage: "https://scholar.google.com/citations?user=voqw10cAAAAJ&hl=en",
    address_flag: "1,✉"
  },
]

// 地址清单（包含地址名称、logo、地址序号）
const addresses = [
  {
    address_flag: "1",
    name: "Peking University",
    icon: "./icon/pku.png",
  },
  {
    address_flag: "2",
    name: "University of Washington",
    icon: "./icon/uw.svg",
  },
  {
    address_flag: "3",
    name: "Beijing Innovation Center of Humanoid Robotics",
    icon: "./icon/x_humanoid.png",
  },
]

// 共一、项目负责人和通讯作者提示
const author_notes = [
  "*: Equal Contribution.",
  "†: Project Lead.",
  "✉: Corresponding Author."
]

// 强调内容，没有则保持空数组
const emphases: string[] = []

// 提供引导资料链接
const buttons = [
  {
    disabled: false,
    name: "Paper",
    link: "#",
    component: Document,
  },
  {
    disabled: false,
    name: "Code",
    link: "#",
    component: Files,
  },
]
</script>

<template>
  <div>

    <!-- 文章logo -->
    <el-row v-if="logo" justify="center">
      <el-image :src="logo" class="logo" fit="cover" />
    </el-row>

    <!-- 文章标题 -->
    <el-row justify="center">
      <el-col :span="20">
        <h1 class="paper-title">
          <span v-if="title" :style="{ color: title_color }">{{ title }}</span>
          <span v-if="title_supp" :style="{ color: title_supp_color }">{{ title_supp }}</span>
        </h1>
      </el-col>
    </el-row>

    <!-- 作者名单 -->
    <el-row justify="center" class="author-row">
      <a
        v-for="author in authors"
        :key="author.name"
        :href="author.homepage"
      >
        <el-button class="title-button" type="primary" text>
          <el-avatar v-if="author.icon" :size="40" :src="author.icon" />
          <span class="author">
            {{ author.name }}<sup v-if="author.address_flag" class="name_sup">{{ author.address_flag }}</sup>
          </span>
        </el-button>
      </a>
    </el-row>

    <!-- 地址名单 -->
    <el-row justify="center" class="address-row">
      <div
        v-for="address in addresses"
        :key="address.address_flag"
        class="address-item"
        :class="{ 'address-item-second-row': address.address_flag === '3' }"
      >
        <img
          v-if="address.icon"
          class="address-icon"
          :src="address.icon"
          :alt="address.name"
        />
        <span class="address">
          <sup v-if="address.address_flag" class="address_sup">{{ address.address_flag }}</sup>{{ address.name }}
        </span>
      </div>
    </el-row>

    <!-- 共一、项目负责人和通讯作者提示内容 -->
    <el-row justify="center" class="con-cor">
      <span
        v-for="note in author_notes"
        :key="note"
        class="con-note"
      >
        {{ note }}
      </span>
    </el-row>

    <!-- 强调内容 -->
    <el-row
      v-for="emphasis in emphases"
      :key="emphasis"
      justify="center"
      class="emphasis"
    >
      {{ emphasis }}
    </el-row>

    <!-- 提供引导按钮 -->
    <el-row justify="center" style="margin-bottom: 20px;">
      <el-col :span="20">
        <el-row justify="center">
          <a
            v-for="button in buttons"
            :key="button.name"
            :href="button.link || undefined"
          >
            <el-button
              class="guidance-button"
              size="default"
              :color="btn_color"
              :disabled="button.disabled"
              round
            >
              <el-icon :size="18">
                <component :is="button.component" />
              </el-icon>
              <span class="btn-text">{{ button.name }}</span>
            </el-button>
          </a>
        </el-row>
      </el-col>
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

/* 作者行：与地址行拉开距离 */
.author-row {
  margin-bottom: 16px;
}

/* 姓名按钮 */
.title-button {
  margin: 10px 3px;
}

/* 姓名按钮光标悬浮 */
.title-button:hover {
  margin: 10px 8px;
}

/* 引导材料按钮 */
.guidance-button {
  margin: 8px 5px;
  box-shadow: #d8d8d8 1px 1px 1px 1px;
}

/* 姓名属性 */
.author {
  font-size: 24px;
  margin-left: 5px;
}

/* 姓名上标属性 */
.name_sup {
  color: #606266;
  margin-left: 3px;
}

/* 地址行：前两个单位第一行，第三个单位第二行居中 */
.address-row {
  margin-top: 36px;
  row-gap: 10px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

/* 单个地址项 */
.address-item {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin: 4px 18px;
  color: #000000;
}

/* 第三个单位单独放到第二行 */
.address-item-second-row {
  flex-basis: 100%;
  margin-top: 2px;
}

/* 地址属性 */
.address {
  font-size: 20px;
  color: #000000;
  line-height: 1.3;
}

/* 地址上标属性 */
.address_sup {
  color: #000000;
  margin-right: 2px;
  font-size: 12px;
}

/* 地址logo属性：不裁剪成圆形，按原始比例显示 */
.address-icon {
  width: 120px;
  height: 50px;
  object-fit: contain;
  border-radius: 0;
  margin-right: 6px;
  vertical-align: middle;
}

/* 头像属性 */
.el-avatar {
  margin-right: 6px;
  box-shadow: #b7b7b7 0px 0px 3px 1px;
}

/* 共一、项目负责人和通讯文字属性 */
.con-cor {
  font-family: Arial;
  font-size: 20px;
  margin: 38px 0px;
  text-align: center;
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 96px;
}

/* 共一、项目负责人和通讯作者单项 */
.con-note {
  color: #000000;
  white-space: nowrap;
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

/* 手机端链接样式处理 */
a:-webkit-any-link {
  text-decoration: none;
}

/* 取消鼠标焦点悬浮在链接上的颜色装饰 */
a:hover {
  color: inherit;
  border-bottom: none;
}

/* 链接装饰，取消下划线和链接颜色 */
a {
  text-decoration: none;
  color: inherit;
}

</style>