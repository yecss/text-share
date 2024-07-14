<template>
  <div>
    <div>
      <form>
        <div class="form-group">
          <label for="comment">输入区:</label>
          <textarea
            v-model="inpData"
            class="form-control"
            rows="5"
            id="comment"
            placeholder="输入需要发布的数据，然后点击发布。"
          ></textarea>
        </div>
        <button
          @click.prevent="sendData"
          class="btn btn-lg btn-primary btn-block"
        >
          发布
        </button>
      </form>
    </div>
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
</style>