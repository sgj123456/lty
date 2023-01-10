<script setup>
import LrcVue from "./components/Lrc.vue"
import music from "../public/data/music.json";
import { onMounted, ref } from "vue";
const tapclick = (TabsPaneContext, Event) => {
  audio.value = `data/mp3/${music[TabsPaneContext.index - 1]?.link_url}`
}
const audio = ref(undefined)
const img = ref("data/img/lty(0).png")
const color = ref(10)
onMounted(
  () => {
    const variation = {
      img: {
        "start": 0,
        "end": 10,
        "step": 1,
        "time": 3000,
        "relapse": true,
        fun(now) {
          img.value = `data/img/lty(${now}).png`
        }
      },
      color: {
        "start": 30,
        "end": 60,
        "step": 1,
        "time": 25,
        "relapse": true,
        fun(now) {
          color.value = now
        }
      }
    }
    Object.keys(variation).forEach(v => {
      guodu(variation[v])
    })
  }
)
function guodu({ start, end, step, time, relapse, fun }) {
  let now = start
  if (relapse) {
    setInterval(() => {
      fun(now)
      if (now >= end && step > 0 || now <= start && step < 0) {
        step = -step
      }
      now += step
    }, time)
  } else {
    setInterval(() => {
      if (now >= end) {
        now = start
      }
      fun(now)
      now += step
    }, time)
  }
}
function paneClick() {
  console.log("demo");
}
</script>
<template>
  <div id="background"
    :style="{ position: 'fixed', backgroundImage: `linear-gradient(0deg, #00ffccaa, #66ccff ${color}%, #ee0000aa)`, zIndex: '-10' }">
  </div>
  <el-tabs tab-position="left" :style="{ height: 'fit-content' }" @tab-click="tapclick" class="demo-tabs">
    <el-tab-pane label="主页" class="pane">
      <h1 style="text-shadow: none;">华风夏韵 洛水天依</h1>
      <strong style="text-shadow: none;"> <em>——</em> 洛天依诞生<em>10</em>周年纪念</strong>&nbsp&nbsp
    </el-tab-pane>
    <el-tab-pane :label="value.name" v-for="(value) of music" class="pane">
      <LrcVue v-bind="value"></LrcVue>
    </el-tab-pane>
  </el-tabs>
  <img :src="img" style="height: 50rem;position: fixed;right: 0;top: 15rem;z-index: -1;" />
  <audio :src="audio" autoplay></audio>
</template>
<style>
.demo-tabs>.el-tabs__content {
  height: 100%;
  color: #66ccff;
  font-weight: 900;
  text-align: right;
}

.el-tabs--left .el-tabs__header.is-left {
  float: right;
  margin-bottom: 0;
  margin-right: 0;
  text-align: left;
  width: 8rem;
  font-size: medium !important;
}

.el-tabs--right .el-tabs__content,
.el-tabs--left .el-tabs__content {
  font-size: 1.25rem;
  font-weight: 700;
  height: 100%;
  width: 100%;
}

.el-tabs__item {
  text-align: left !important;
  padding: 0 0.5rem;
  height: var(--el-tabs-header-height);
  box-sizing: border-box;
  line-height: var(--el-tabs-header-height);
  display: inline-block;
  list-style: none;
  font-size: var(--el-font-size-base);
  font-weight: 700;
  color: var(--el-text-color-primary);
  position: relative;
}

.demo-tabs {
  height: 100% !important;
  display: flex;
}

#background {
  height: 100%;
  width: 100%
}

.pane {
  height: 100% !important;
}
</style>
