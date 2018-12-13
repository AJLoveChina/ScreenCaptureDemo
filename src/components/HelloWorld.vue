<template>
  <div class="div">
    <button @click="capture">Capture Screen</button>
    <p>如果你使用的是Chrome， 请先 ： 扩展程序 => 加载已解压的扩展程序 => 选择项目的根目录下面的chrome-extension-sample文件夹 => 完成（记得开启开发者模式）</p>
    <p>建议分享桌面的时候不要选择桌面， 而是选择应用， 这样不会出现画中画。</p>
    <p>WebRTC相关的API大部分需要走https访问（不安全的https是允许的，比如证书错误）</p>
    <p>获取桌面视频流的兼容性问题 ： 截止2018年12月13日</p>
    <ul>
      <li>firefox，Edge可以开箱即用， 其中win10家庭版Edge浏览器不支持</li>
      <li>Chrome需要插件， 未来版本的Chrome计划不依赖插件</li>
      <li>Safari暂时不支持，目前也提上日程了</li>
    </ul>
    <br>
    <video ref="video" playsinline autoplay></video>
  </div>
</template>

<script>
  import getscreenmedia from 'getscreenmedia'

  export default {
    name: 'HelloWorld',
    data() {
      return {
        msg: 'Welcome to Your Vue.js App'
      }
    },
    mounted() {
      this.bindEvent();
    },
    methods: {
      bindEvent() {
        let video = this.$refs.video;
        video.addEventListener("loadedmetadata", () => {
          console.log(video.videoWidth, video.videoHeight);
        })
      },
      capture() {
        getscreenmedia((err, stream) => {
          if (err) {
            console.log('failed', err);
          } else {
            try {
              this.$refs.video.srcObject = stream;
            } catch (error) {
              this.$refs.video.src = URL.createObjectURL(stream);
            }
          }
        });
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .div{
    font-size:12px;
  }
</style>
