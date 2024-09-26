<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount } from 'vue';
import Header from '../components/Header.vue'; // 引入 Header 组件
import coolCat from '../assets/coolCat.png'; // 引入图片
import coolCat2 from '../assets/coolCat2.png'; // 引入图片
import arrow from '../assets/icons8-down-arrow-26.png';
import {Download, Location, Message, Phone} from "@element-plus/icons-vue"; // 引入箭头图片
const activeSection = ref('home'); // 当前激活部分

let observer = null;
const scrollToAbout = () => {
  const aboutSection = document.getElementById('about');
  if (aboutSection) {
    aboutSection.scrollIntoView({ behavior: 'smooth' });
  }
};

const scrollToContact = () => {
  const contactSection = document.getElementById('contact');
  if (contactSection) {
    contactSection.scrollIntoView({ behavior: 'smooth' });
  }
};
onMounted(() => {
  const sections = document.querySelectorAll('section');

  // 使用 Intersection Observer 监控每个 section
  observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            activeSection.value = entry.target.id; // 更新当前激活部分
          }
        });
      },
      {
        threshold: 0.6, // 当 60% 的部分进入视口时触发
      }
  );

  sections.forEach((section) => {
    observer.observe(section);
  });
});
const downloadResume = () => {
  const url = import('../static/resume.pdf'); // 使用 require 来获取文件路径
  const link = document.createElement('a');
  link.href = url;
  link.download = 'resume.pdf'; // 设置下载的文件名
  link.click();
};
onBeforeUnmount(() => {
  if (observer) {
    observer.disconnect();
  }
});
const expandedIndex = ref(null);
const customColor = 'orange'; // 自定义颜色
const skills = ref([
  {
    title: '前端技能',
    skills: [
      { name: 'HTML/CSS', percentage: 80 },
      { name: 'JavaScript', percentage: 50 },
      { name: 'TypeScript', percentage: 30 },
      { name: 'Vue.js', percentage: 40 },
    ],
  },
  {
    title: '后端技能',
    skills: [
      { name: 'JAVA', percentage: 70 },
      { name: 'MySQL', percentage: 60 },
      { name: 'Git', percentage: 50 },
    ],
  },
]);

const toggle = (index) => {
  expandedIndex.value = expandedIndex.value === index ? null : index;
};



</script>

<template>
  <Header class="header-container" :activeSection="activeSection" />
  <div class="container">
    <section id="home">
      <div class="home-container">
        <div class="home-left">
          <el-text class="home-title">
            Hi, I'm CoolCat
          </el-text>
          <p class="job-title">前端工程师</p>
          <p class="intro-text">
            热爱学习，热爱生活，创造更高质量工作成果。
          </p>
          <div class="home-buttons">
            <el-button class="contact-button" color="orange" size="large" @click="scrollToContact">
              联系我
            </el-button>
          </div>
        </div>
        <div class="home-right">
          <div class="image-container">
            <img :src="coolCat" alt="coolCat" class="cool-cat-img">
          </div>
        </div>

      </div>
      <div style="margin-top: 40px;" @click="scrollToAbout">
        <img :src="arrow" alt="arrow" class="arrow-img">往下滑
      </div>
    </section>

    <section id="about">
      <div style="display: flex; flex-direction: column; ">
        <el-text class="section-title">关于我</el-text>
        <el-text class="section-text">简单介绍</el-text>
      </div>

      <div class="about-container">
        <div class="about-left">
          <img :src="coolCat2" alt="coolCat2" class="cool-cat-img">
        </div>
        <div class="about-right">
          <p class="about-text">Web开发人员，具有丰富的知识和多年的经验，从事Web技术和后端开发，提供高质量的工作。</p>
          <div class="about-stats">
            <div class="about-stat">
              <p>1年内经验</p>
            </div>
            <div class="about-stat">
              <p>1+已完成项目</p>
            </div>
            <div class="about-stat">
              <p>2+公司任职</p>
            </div>
          </div>
          <div class="about-button">
            <el-button class="download-button" color="orange" size="large" @click="downloadResume">
              下载简历
              <el-icon style="margin-left: 10px;"><Download /></el-icon>
            </el-button>
          </div>
        </div>
      </div>
    </section>
    <section id="skills">
      <div style="display: flex; flex-direction: column; ">
        <el-text class="section-title">技能</el-text>
        <el-text class="section-text">技能水平</el-text>
      </div>
      <div style="margin-top: 20px;">
        <div v-for="(item, index) in skills" :key="index" style="margin-bottom: 40px; ">
          <el-text @click="toggle(index)" class="clickable">{{ item.title }}</el-text>
          <el-collapse accordion v-model="expandedIndex">
            <el-collapse-item class="skill-collapse" :name="index.toString()">
              <div v-for="(skill, i) in item.skills" :key="i">
                <el-text>{{ skill.name }}</el-text>
                <el-progress
                    :percentage="skill.percentage"
                    :color="customColor">
                </el-progress>
              </div>
            </el-collapse-item>
          </el-collapse>
        </div>
      </div>
    </section>
    <section id="contact">
      <div style="display: flex; flex-direction: column; ">
        <el-text class="section-title">联系我</el-text>
        <el-text class="section-text">你可以通过以下方式联系我:</el-text>
      </div>
      <div class="contact-container">
        <div class="contact-item">
          <el-icon class="contact-icon" :color="customColor">
            <Phone />
          </el-icon>
          <div class="contact-info">
            <el-text class="contact-title">电话</el-text>
            <el-text class="contact-text">13225999295</el-text>
          </div>
        </div>
        <div class="contact-item">
          <el-icon class="contact-icon" >
            <Message />
          </el-icon>
          <div class="contact-info">
            <el-text class="contact-title">邮箱</el-text>
            <el-text class="contact-text">13225999295@163.com</el-text>
          </div>
        </div>
        <div class="contact-item">
          <el-icon class="contact-icon" >
            <Location />
          </el-icon>
          <div class="contact-info">
            <el-text class="contact-title">地点</el-text>
            <el-text class="contact-text">福建省</el-text>
          </div>
        </div>
      </div>

    </section>
  </div>
  <el-backtop :right="50" :bottom="50" style="color: orange;" />
</template>

<style scoped>
.header-container {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 999;
  max-width: 1000px;
  margin: 0 auto;
}
.container
{
  max-width: 800px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

}
.container section
{
  padding-top: 12vh;
  width: 100%;

}
   /* 布局样式 */
.home-container {
  width: 100%;
  display: grid;
  grid-template-columns: 2fr 1fr;
 }

/* 左侧部分样式 */
.home-left {
  display: grid;
  grid-template-rows: 1fr 1fr 1fr;
}
.home-left button {
  font-weight: normal;
  font-size: clamp(1rem, 1vw, 1.2rem); /* 动态字体 */
  border-radius: 10px; /* 圆角 */
  width: auto; /* 自适应宽度 */
  height: auto; /* 自适应高度 */
}
/* 标题样式 */
.home-title {
  font-size: clamp(2rem, 2.2vw, 2.4rem); /* 视口宽度自适应字体大小 */
  font-weight: bold;
  color: black;
}

/* 职业描述样式 */
.job-title {
  font-size: clamp(1.5rem, 1.8vw, 2rem); /* 动态调整字体大小 */
  color: black;
  margin: 10px 0;
}

/* 简介文本样式 */
.intro-text {
  font-size: clamp(1rem, 1.2vw, 1.5rem);
  color: gray;
  margin-bottom: 20px;
}

.home-buttons {
  display: flex;
  width: 30%;
  margin-top: auto;
}
.contact-button {
  font-weight: normal;
  font-size: clamp(1rem, 1.1vw, 1.2rem); /* 动态字体 */
  border-radius: 10px; /* 圆角 */
  width: auto; /* 自适应宽度 */
  height: auto; /* 自适应高度 */
  padding: 10px 20px; /* 内边距 */
  white-space: nowrap; /* 防止文本换行 */
}

/* 右侧部分样式 */
.home-right {
  flex: 1;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* 图片容器 */
.image-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* 图片样式 */
.cool-cat-img {
  width: clamp(150px, 15vw, 500px);
  height: auto;
  object-fit: cover;
}

.about-container {
  margin-top: 20px;
  display: grid;
  grid-template-columns: 1fr 2fr;
  grid-gap: 30px;
}
.about-left {
  display: flex;
  justify-content: center;
  align-items: center;
}

.section-title {
  font-size: clamp(2rem, 2.2vw, 2.4rem);
  font-weight: bold;
  color: black;
}
.section-text {
  font-size: clamp(1rem, 1vw, 1.8rem);
  color: gray;
  margin-top: 10px;
}
.about-right {
  flex: 1;
  display: flex;
  flex-direction: column;

}
.about-text {
  font-size: clamp(1.2rem, 1.5vw, 1.8rem);
  color: black;

}
.about-stats {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}
.about-stat {
  display: flex;
  margin: auto;
  flex-direction: column;
  align-items: center;
}
.about-stat p {
  font-weight: lighter;
  font-size: clamp(1rem, 1.2vw, 1.5rem);
  color: gray;

}
.about-button {
  display: flex;
  width: 30%;
  margin-top: auto;
}
.download-button {
  font-weight: normal;
  font-size: clamp(1rem, 1.2vw, 1.5rem); /* 动态字体 */
  border-radius: 10px; /* 圆角 */
  width: auto; /* 自适应宽度 */
  height: auto; /* 自适应高度 */
  padding: 10px 20px; /* 内边距 */
  white-space: nowrap; /* 防止文本换行 */
}

.clickable {
  cursor: pointer; /* 鼠标悬停时显示为可点击 */
  font-weight: bolder; /* 加粗 */
  color: black; /* 黑色 */
  font-size: 20px; /* 动态调整字体大小 */
}
.contact-container {
  justify-content: center;
  align-items: center;
  margin-top: 20px;
}

.contact-item {
  display: flex;
  align-items: center;
  margin-bottom: 30px;
}
.contact-icon {
  font-size: clamp(2.8rem, 3vw, 3.8rem);
  color: orange;
}
.contact-info{
  margin-left: 50px;
  display: grid;
  grid-template-rows: 1fr 1fr;
}
.contact-title {
  font-weight: bold;
  font-size: clamp(1.2rem, 1.4vw, 2rem);
  color: black;
}
.contact-text {
  font-size: clamp(1.2rem, 1.2vw, 1.3rem);
  color: gray;
}

</style>
