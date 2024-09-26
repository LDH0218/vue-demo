<script setup lang="ts">
import {Moon, Sunny} from "@element-plus/icons-vue";
import { ref } from 'vue';


const theme = ref("light");
const props = defineProps<{
  activeSection: string;
}>();

const scrollToSection = (id: string) => {
  const section = document.getElementById(id);
  if (section) {
    section.scrollIntoView({ behavior: 'smooth' });
  }
};
const toggleTheme = () => {
  if (theme.value === "light") {
    theme.value = "dark";
  } else {
    theme.value = "light";
  }
};
</script>

<template>
  <div class="header">
    <div class="logo">
      <img src="/src/assets/logo.png" alt="logo">
      <span style="font-size: 20px;font-weight: bold; margin-left: 10px;" >橘子</span>
    </div>
    <div style="flex: 1;"/>
    <div class="nav-container">
      <div class="nav">
        <a
            href="#home"
            :class="{ active: activeSection === 'home' }"
            @click.prevent="scrollToSection('home')"
        >主页</a>
        <a
            href="#about"
            :class="{ active: activeSection === 'about' }"
            @click.prevent="scrollToSection('about')"
        >关于</a>
        <a
            href="#skills"
            :class="{ active: activeSection === 'skills' }"
            @click.prevent="scrollToSection('skills')"
        >技能</a>
        <a
            href="#contact"
            :class="{ active: activeSection === 'contact' }"
            @click.prevent="scrollToSection('contact')"
        >联系</a>
      </div>
      <el-switch
          style="margin-left: 2rem;"
          v-model="theme"
          :active-action-icon="Sunny"
          :inactive-action-icon="Moon"
      />
    </div>
  </div>
</template>
<style scoped>
.logo {
  display: flex;
  align-items: center;
  justify-content: center;
}
.logo img {
  width: 50px;
}
.header {
  margin-left: auto;
  margin-right: auto;
  padding-top: 10px;
  padding-bottom: 10px;
  background-color: #fff;
  color: black;
  display: flex;
  align-items: center;
  justify-content: center;
}

.nav-container{
  display: flex;
  align-items: center;
}
.nav {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 1rem;
  font-size: 16px;
}
.nav a {
  font-weight: bold;
  color: #333; /* 将字体颜色应用到 a 标签 */
  text-decoration: none; /* 去掉 a 标签的下划线 */
}
.nav a:hover {
  color: #fe8f1f; /* 鼠标悬停时字体颜色 */
}
.nav a.active {
  color: #fe8f1f; /* 激活状态下的颜色 */

}
</style>
