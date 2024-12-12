<script setup>
import { RouterView } from 'vue-router'
import { GithubFilled, HomeOutlined } from '@ant-design/icons-vue'
function openGitHub() {
  window.open("https://github.com/TeasOfficial/fastdl-website")
}
</script>

<template>

  <Divider :border-width="3" border-color="blue">
    <GradientText type="info" :size="24" :weight="700">
      Fastdl.me 中文网 | 地图下载中心
    </GradientText>
  </Divider>

  <RouterView />

  <Divider :border-width="3" border-color="red">
    <GradientText type="error" :size="24" :weight="700">
      Teas Official
    </GradientText>
  </Divider>

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
</template>

<style scoped>
</style>
