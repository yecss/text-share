<template>
  <!-- <Demo></Demo> -->
  <div class="wrapper container">
    <Display></Display>
    <Submit></Submit>
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