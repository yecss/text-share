<template>
  <h3>Hello Demo!</h3>
  <button @click="saveData">保存数据</button>
  <br />
  <button @click="deleteData">删除数据</button>
  <br />
  <button @click="changeData">修改数据</button>
  <br />
  <button @click="readData">查询数据</button>
</template>

<script>
const AV = require("leancloud-storage");
const { Query, User } = AV;
// AV.init({
//   appId: "jbej1lm8yk50Pim43tiN181l-9Nh9j0Va",
//   appKey: "gdQkCmbsoODB5pmCm7gVR9UW",
//   serverURL: "https://jbej1lm8.lc-cn-e1-shared.com",
// });
export default {
  name: "Demo",
  setup() {
    function saveData() {
      const TestObject = AV.Object.extend("TestObject");
      const testObject = new TestObject();
      testObject.set("words", "Hello world!");
      testObject.save().then((testObject) => {
        console.log("保存成功。");
      });
    }
    function deleteData() {
      // 删除TestObject下的id为62dd349e021a1f1f40f6aee3的对象
      const todo = AV.Object.createWithoutData(
        "TestObject",
        "62dd349e021a1f1f40f6aee3"
      );
      todo.destroy();
    }
    function changeData() {
      const todo = AV.Object.createWithoutData(
        "TestObject",
        "62dd3685021a1f1f40f6af4d"
      );
      todo.set("content", "这周周会改到周三下午三点。");
      todo.save();
    }
    function readData() {
      // const query = new AV.Query("TestObject");
      // query.get("62dd3684021a1f1f40f6af4b").then((todo) => {
      //   // todo 就是 objectId 为 582570f38ac247004f39c24b 的 Todo 实例
      //   const words = todo.get("title");
      //   // 获取内置属性
      //   const objectId = todo.id;
      //   const updatedAt = todo.updatedAt;
      //   const createdAt = todo.createdAt;
      // });
      const query = new AV.Query('TestObject');
      query.get('62dd3684021a1f1f40f6af4b').then((todo) => {
        console.log(todo.toJSON())
      })
      // 一次获取所有属性
      
    }
    return {
      saveData,
      deleteData,
      changeData,
      readData
    };
  },
};
</script>

<style>
</style>