<template>
  <!-- <Demo></Demo> -->
  <div class="wrapper container">
    <Display></Display>
    <Submit></Submit>
    <footer>
    <p>Developed by 海边的浪. <a href="https://github.com/yecss/text-share" target="_blank">GitHub</a>.</p>
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
</style>