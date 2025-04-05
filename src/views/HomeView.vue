<script>
import { ref } from 'vue';
import axios from 'axios';
export default {
  data() {
    return {
      servers: [],
      getted: ref(false)
    }
  },
  methods: {
    viewall() {
      this.$router.push('/maps')
    },
    download(mapname) {
      const iframe = document.createElement('iframe');
      iframe.style.display = 'none';
      iframe.src = 'https://main.fastdl.me/maps/' + mapname + '.bsp.bz2';
      document.body.appendChild(iframe);
    }
  },
  mounted() {
    axios.get("https://nekogan.com/servers/bhop.php").then(
      (res) => {
        this.servers = res.data,
          this.getted = true
      }
    )
  }
}
</script>

<template>
  <Card @click="viewall()" hoverable title="预览全部地图">
    <b>View all maps</b>
  </Card>
  <Divider :border-width="3" border-color="orange">
    <GradientText type="warning" :size="16" :weight="700">
      下载服务器当前地图
    </GradientText>
  </Divider>
  <Spin :spinning="!getted" indicator="magic-ring">
    <div v-if="!getted">
      <p class="spin-content">
        <center>
          <br>
          <br>
          <br>
          正在获取服务器信息，请稍后...
          <br>
          <br>
          <br>
        </center>
      </p>
    </div>
  </Spin>

  <span v-for="v in servers">
    <br>
    <Card hoverable :title="v.HostName" @click="download(v.Map)">
      <template #extra>
        {{ v.Players }} / {{ v.MaxPlayers }}
      </template>
      <p>当前地图：{{ v.Map }}</p>
    </Card>
  </span>
</template>
