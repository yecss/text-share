<template>
  <!-- <Demo></Demo> -->
  <div class="wrapper container">
    <h2 class="title">在线文本速传</h2>
    <p class="mt-4 introduce">🐳描述：我们常常遇到这样的情况：想要将手机上的一段文本传输到电脑，通常的操作是复制文本、打开微信、粘贴并发送到文件传输助手，然后再在电脑上接收。现在，无需再打开微信，只需访问这个网站，就能轻松完成文本传输！</p>
    <Submit></Submit>
    <Display></Display>
    
    <footer>
    <p>Developed by 海边的浪 2025 <a href="https://github.com/yecss/text-share" target="_blank">GitHub</a></p>
  </footer>
  </div>
</template>

<script>
import Demo from "./components/Demo.vue";
import Submit from "./components/Submit.vue";
import Display from "./components/Display.vue";
export default {
  name: "App",
  components: {
    Demo,
    Submit,
    Display,
  },
};
</script>

<style>
@media screen and (min-width: 768px) {
  .wrapper {
    width: 50%;
  }
}
body{
  font-family: 'Microsoft Yahei';
}
.title{
  margin-top: 30px;
  margin-bottom: 30px;
  text-align: center;
}
.introduce{
  font-size: 1rem;
  color: #515151;
}
</style>