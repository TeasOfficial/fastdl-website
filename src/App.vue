<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
import { RouterView } from 'vue-router'
import { GithubFilled, HomeOutlined } from '@ant-design/icons-vue'
import { toggleDark, useMutationObserver } from 'vue-amazing-ui'
function openGitHub() {
  window.open("https://github.com/TeasOfficial/fastdl-website")
}
const notification = ref()
const message = ref()
const themeDark = ref()
onMounted(() => {
  const themeMedia = window.matchMedia("(prefers-color-scheme: light)");
  if (!themeMedia.matches) {
    darkModeSwitch()
  }

  window['$message'] = message.value
  window['$notification'] = notification.value
})
onBeforeUnmount(() => {
  delete window['$notification']
  delete window['$message']
});
function darkModeSwitch(){
  toggleDark()
  themeDark.value = document.documentElement.classList.contains('dark')
}
</script>

<template>
  <Divider :border-width="3" border-color="blue">
    <GradientText type="info" :size="24" :weight="700">
      Fastdl.me 中文网 | 地图下载中心
    </GradientText>
  </Divider>

  <Notification ref="notification" />
  <RouterView />
  <Message ref="message" />

  <Divider :border-width="3" border-color="red">
    <GradientText type="error" :size="24" :weight="700">
      Teas Official
    </GradientText>
  </Divider>

  <center class="watermark">
    <span style="color: gainsboro;">
      <i>Website made by NekoGan</i>
      <br>
      <i>Last Update: 2025/04/05</i>
    </span>
  </center>

  <FloatButton shape="circle" tooltip="GitHub" type="primary" @click="openGitHub">
    <template #icon>
      <GithubFilled spin />
    </template>
  </FloatButton>
  <FloatButton v-if="$route.path != '/'" shape="circle" tooltip="返回主页" ghost :bottom='100' @click="$router.push('/')">
    <template #icon>
      <HomeOutlined />
    </template>
  </FloatButton>
  <span v-if="$route.path == '/'">
    <FloatButton v-if="themeDark" shape="circle" tooltip="明亮模式" :bottom='100' @click="darkModeSwitch">
      <template #icon>
        <img style="width: 1em; height: 1em; color: white;" src="./assets/sun.svg" />
      </template>
    </FloatButton>
    <FloatButton v-else shape="circle" tooltip="暗黑模式" :bottom='100' @click="darkModeSwitch">
      <template #icon>
        <img style="width: 1em; height: 1em" src="./assets/moon.svg" />
      </template>
    </FloatButton>
  </span>
</template>

<style lang="less" scoped>
.u-theme-switch {
  :deep(.switch-checked) {
    background: #faad14;

    &:hover:not(.disabled) {
      background: #e8b339;
    }
  }

  .u-dark-svg {
    width: 12px;
    height: 12px;
    fill: #fff;
  }

  .u-light-svg {
    width: 12px;
    height: 12px;
    fill: rgba(60, 60, 67, .75);
  }
}
</style>
