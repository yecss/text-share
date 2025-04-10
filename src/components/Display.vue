<template>
  
  <div class="card mb-4 mt-4">
    <div class="card-header">
      <span class="my-0 font-weight-normal">展示区:</span>
      <button class="btn btn-outline-danger" @click="clearAll">清空</button>
    </div>
    <div class="card-body">
      <div class="list-group list-group-flush">
        <div v-for="(i, index) in rData" :key="index" class="list-group-item">
          <div class="item-header">
            <p class="create-time">创建时间: {{ i.createdAt }}</p>
            <button class="btn btn-outline-primary copy-btn" @click="copyText(i.text)">复制</button>
          </div>
          <pre class="list-group-item" :data-index="index + 1">{{ i.text }}</pre>
        </div>
      </div>
    </div>
  </div>
  
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
      const password = prompt("请输入密码：");
      if (password !== "admin") {
        alert("密码错误，操作已取消");
        return;
      }

      if (confirm("确定要清空展示区吗？")) {
        // 批量删除
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
        rData.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt)); // 按时间排序
      });
      Bus.on("updataDisplay", (v) => {
        rData.push({
          text: v,
          createdAt: new Date().toLocaleString()
        });
        rData.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt)); // 按时间排序
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
pre.list-group-item {
  padding-left: 2.6rem;
  background-color: #f1f3f5;
  border-radius: 5px;
  padding: 10px;
  margin-top: 5px;
  font-size: 14px;
  border: 1px solid #e0e0e0;
}
.btn {
  float: right;
  margin-top: 10px;
}
.copy-btn {
  font-size: 12px;
  /* margin-right: 10px; */
  margin-top: 0;
}
.item-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 5px;
}
.list-group-flush .list-group-item {
  border-top-width: 0;
  padding: 10px 15px;
}
.list-group-flush .list-group-item:first-child {
  border-bottom-width: 0;
}
pre {
  font-family: 'Microsoft YaHei', sans-serif;
  margin-bottom: 0.1rem;
}
.text-muted {
  font-size: 70%;
  color: #000000;
}
.card {
  /* box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); */
  border-radius: 10px;
  background-color: #ffffff;
}
.card-header{
  line-height: 45px;
}
.card-header p {
  
  margin: 0;
  font-size: 16px;
  font-weight: bold;
}
.card-body {
  padding: 15px;
}
.create-time{
  font-size: 12px;
  color: brown;
  line-height: 30px;
}
</style>