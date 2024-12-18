<template>
  <p class="mt-4 ">👍前言：我们常常遇到这样的情况：想要将手机上的一段文本传输到电脑，通常的操作是复制文本、打开微信、粘贴并发送到文件传输助手，然后再在电脑上接收。现在，无需再打开微信，只需访问这个网站，就能轻松完成文本传输！</p>
  <div class="card mb-4">
    <div class="card-header">
      <p class="my-0 font-weight-normal">展示区:</p>
    </div>
    <div class="card-body">
      <div class="list-group list-group-flush">
        <div v-for="(i, index) in rData" :key="index" class="list-group-item">
          <button class="btn btn-outline-primary copy-btn" @click="copyText(i.text)">复制</button>
          <pre class="list-group-item" :data-index="index + 1">{{ i.text }}</pre>
          <small class="text-muted">创建时间: {{ i.createdAt }}</small>
        </div>
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
      if (confirm("确定要清空展示区吗？")) {
        //   批量删除
        const arr1 = [...idData];
        for (let i = 0; i < arr1.length; i++) {
          const todo = AV.Object.createWithoutData("messageBoard", arr1[i]);
          todo.destroy();
          this.rData.pop();
        }
        alert("清除成功");
      }
    }
    function copyText(text) {
      navigator.clipboard.writeText(text).then(() => {
        alert("复制成功");
      }).catch(err => {
        alert("复制失败");
      });
    }
    onMounted(() => {
      const query = new AV.Query("messageBoard");
      query.find().then((v) => {
        v.forEach((element) => {
          idData.push(element.id);
          rData.push({
            text: element.attributes.words,
            createdAt: new Date(element.createdAt).toLocaleString()
          });
        });
      });
      Bus.on("updataDisplay", (v) => {
        rData.push({
          text: v,
          createdAt: new Date().toLocaleString()
        });
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
      copyText,
    };
  },
};
</script>

<style scoped>
pre.list-group-item::before {
  content: "💕"attr(data-index)":";
  position: absolute;
  left: 0px;
}
pre.list-group-item{
  padding-left: 2.6rem;
}
.btn{
  float: right;
  margin-top: 10px;
}
.copy-btn{
  font-size: 12px;
}
.list-group-flush .list-group-item{
  border-top-width: 0;
  /* border-bottom-width: 0; */
}
.list-group-flush .list-group-item:first-child {
    border-bottom-width: 0;
}
pre {
    /* margin-bottom: 6px; */
    font-family: 'Microsoft YaHei', sans-serif;
    margin-bottom: 0.1rem;
}
.text-muted{
  font-size: 70%;
}
</style>