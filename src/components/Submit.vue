<template>
  <div class="submit-container">
    <form>
      <div class="form-group">
        <label for="comment" class="form-label">输入区:</label>
        <textarea
          v-model="inpData"
          class="form-control"
          rows="5"
          id="comment"
          placeholder="输入需要发布的数据，然后点击发布。"
        ></textarea>
      </div>
      <button
        @keypress="enter"
        @click.prevent="sendData"
        class="btn btn-lg btn-primary btn-block submit-btn"
      >
        发布
      </button>
    </form>
  </div>
</template>

<script>
const AV = require("leancloud-storage");
const { Query, User } = AV;
AV.init({
  appId: "ZzP4aKPXH1H7lYV6fcqsdmkA-gzGzoHsz",
  appKey: "WuUgNOxJKoQD6XOIu2F2yDKr",
  serverURL: "https://zzp4akpx.lc-cn-n1-shared.com",
});
AV.debug.disable(); // 停用

import Bus from "../plugin/Bus";
import { ref } from "vue";
export default {
  name: "Submit",
  setup() {
    let inpData = ref("");
    function sendData() {
      if (this.inpData) {
        const TestObject = AV.Object.extend("messageBoard");
        const testObject = new TestObject();
        testObject.set("words", inpData.value);
        console.log(inpData.value);
        testObject.save().then((testObject) => {});
        Bus.emit("updataDisplay", inpData.value);
        inpData.value = "";
        alert("数据发送成功");
      }
      else{
        alert('请输入数据!')
      }
    }
    
    return {
      sendData,
      inpData,
    };
  },
};
</script>

<style scoped>
.submit-container {
  margin: 20px auto;
  border: 1px solid rgba(0, 0, 0, .125);
  /* max-width: 600px; */
  padding: 20px;
  background-color: #f8f9fa;
  border-radius: 10px;
  /* box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); */
}
.form-label {
  font-size: 16px;
  /* font-weight: bold; */
  margin-bottom: 10px;
  display: block;
}
.form-control {
  font-size: 14px;
  padding: 10px;
  border: 1px solid #ced4da;
  border-radius: 5px;
  resize: none;
}
.form-control:focus {
  border-color: #80bdff;
  outline: none;
  box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
}
.submit-btn {
  margin-top: 15px;
  background-color: #007bff;
  border: none;
  color: white;
  padding: 10px 20px;
  font-size: 16px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}
.submit-btn:hover {
  background-color: #0056b3;
}
</style>