<template>
  <p class="mt-4 ">👍前言：我们经常会遇到这样的情况：想要从手机上传输一段文本到电脑上时，我们的操作往往是，复制文本，打开手机微信，粘贴发送到文件传输助手，接着打开电脑微信，接收文本。而现在，我们不需要打开微信。只需要打开这个网站！</p>
  <div class="card mb-4">
    <div class="card-header">
      <p class="my-0 font-weight-normal">展示区:</p>
    </div>
    <div class="card-body">
      <div class="list-group list-group-flush">
        <pre v-for="(i, index) in rData" :key="index" class="list-group-item">{{ i }}</pre>
      </div>
    </div>
  </div>
  <button class="btn btn-outline-danger mb-4" @click="clearAll">清空展示区</button>
  <br>
  <br>
</template>



<script>
const AV = require("leancloud-storage");
const { Query, User } = AV;
AV.debug.disable(); // 停用

import { onBeforeUnmount, onMounted, reactive } from "vue";
import Bus from "../plugin/Bus";
export default {
  name: "Display",
  setup() {
    let rData = reactive([]);
    let idData = reactive([]);

    // function readData() {
    //   const query = new AV.Query("TestObject");
    //   query.get("62dd3684021a1f1f40f6af4b").then((todo) => {
    //     console.log(todo.toJSON());
    //   });
    // 查询
    //   console.log(rData)
    // }
    function clearAll() {
      //   批量删除
      const arr1 = [...idData];
      for (let i = 0; i < arr1.length; i++) {
        const todo = AV.Object.createWithoutData("messageBoard", arr1[i]);
        todo.destroy();
        this.rData.pop();
      }
      alert("清除成功");
    }
    onMounted(() => {
      const query = new AV.Query("messageBoard");
      query.find().then((v) => {
        v.forEach((element) => {
          idData.push(element.id);
        });
        v.forEach((element) => {
          rData.push(element.attributes.words);
        });
      });
      Bus.on("updataDisplay", (v) => {
        rData.push(v);
        const query = new AV.Query("messageBoard");
        query.find().then((v) => {
          v.forEach((element) => {
            idData.push(element.id);
          });
        });
      });
    });
    return {
      rData,
      idData,
      clearAll,
    };
  },
};
</script>

<style scoped>
.btn{
  float: right;
}
</style>