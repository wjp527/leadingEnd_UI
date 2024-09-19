<template>
  <div class="container">
    <div
      v-for="(poker, index) in pokerEles"
      :key="index"
      class="poker"
      :class="'poker' + (index + 1)"
      :style="{ 
        zIndex: poker.nums, 
        transform: poker.transform, 
        transition: poker.transition 
      }"
    >
      <img :src="poker.imgSrc"/>
    </div>
    <div class="poker_top poker5" @click="move"></div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

const pokerEles = ref([]) as any;

// 每张图片的旋转角度和位移数据
const transformDatas = ref([
  'rotate(-10deg)',
  'rotate(-6deg) translate(35%, -12%)',
  'rotate(-2deg) translate(65%, -19%)',
  'rotate(2deg) translate(95%, -26%)',
  'rotate(6deg) translate(125%, -23%)'
]);

const imgs = ref([]) as any;
const imgIndex = ref(0);

const init = () => {
  for (let i = 0; i < 9; i++) {
    imgs.value.push(`/image/photos/photo (${i}).webp`);
  }

  pokerEles.value = Array.from({ length: 5 }, (v, index) => ({
    nums: index,
    imgSrc: `/image/photos/photo (${4 - index}).webp`,
    // 初始化过渡动画
    transform: transformDatas.value[index], // 初始transform
    transition: "transform 0.3s ease" // 初始过渡效果
  }));
};

// 点击卡牌
const move = () => {
  pokerEles.value.forEach((ele : any) => {
    let nums = ele.nums;
    if (nums + 1 >= pokerEles.value.length) {
      nums = 0;
      ele.imgSrc = imgs.value[imgIndex.value];
      imgIndex.value = (imgIndex.value + 1) % imgs.value.length;
    } else {
      nums += 1;
    }

    // 更新 z-index 和 transform 属性，并确保 transition 应用于变换
    ele.nums = nums;
    // 进行平滑滚动
    ele.transform = transformDatas.value[nums];  // 更新 transform
    ele.transition = "transform 0.3s ease";  // 确保 transition 仍然有效
  });
};

onMounted(init);
</script>

<style scoped>
div {
  user-select: none;
}

* {
  font-size: 2vmin;
  margin: 0;
  padding: 0;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
  background-color: #000;
}

.container {
  position: absolute;
  width: 45rem;
  height: 25rem;
  margin-bottom: 1rem;
}

.poker,
.poker_top {
  position: absolute;
  width: 20rem;
  height: 26rem;
  border: 0.15rem solid #fff;
  border-radius: 1.5rem;
  background-color: #17f700;
  transform-origin: bottom left;
  overflow: hidden;
}

.poker img {
  position: relative;
  width: 100%;
}

.poker_top {
  background-color: #fff;
  transition: 0.3s ease;
  cursor: pointer;
  z-index: 1000;
}

.poker_top:hover {
  background-color: #aaa;
}

.poker1 {
  transform: rotate(-10deg);
}

.poker2 {
  transform: rotate(-6deg) translate(35%, -12%);
}

.poker3 {
  transform: rotate(-2deg) translate(65%, -19%);
}

.poker4 {
  transform: rotate(2deg) translate(95%, -26%);
}

.poker5 {
  transform: rotate(6deg) translate(125%, -23%);
}
</style>
