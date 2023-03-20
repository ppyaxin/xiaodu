<template>
  <div>
    <div id="data-list-content" style="height: 400px;width: 100%;overflow-x: hidden;overflow-y: auto;
  background-color: aliceblue;border-radius: 10px;">
      <div v-for="item in messageList" :key="item.id">
        <!-- 左侧 -->
        <div v-if="item.position == 'left'" style="width: 100%;display: flex;padding: 5px;">
          <div style="padding:5px">
            <img src="../assets/xiaodu.png" style="width: 50px;height: 50px;border-radius: 100%;">
          </div>
          <div>
            <div>
              <span style="font-size: 8px;color: #798588;">小杜小杜</span>
            </div>
            <div style="background-color:#0E4A82;max-width: 70%;padding: 4px;border-radius: 5px;color: white;">
              <span>{{ item.message }}</span>
            </div>
          </div>
        </div>
        <!-- 右侧 -->
        <div v-if="item.position == 'right'" style="width: 100%;display: flex;padding: 5px;flex-direction:row-reverse;">
          <div style="padding:5px">
            <img src="../assets/user.png" style="width: 50px;height: 50px;border-radius: 100%;">
          </div>
          <!-- 右侧 -->
          <div>
            <div style="text-align: right;">
              <span style="font-size: 8px;color: #798588;">用户</span>
            </div>
            <div style="display: flex;flex-direction:row-reverse;">
              <div style="background-color: #81BC2B;max-width: 70%;padding: 4px;border-radius: 5px;">
                <span>{{ item.message }}</span>
              </div>
            </div>
          </div>

        </div>
      </div>
    </div>
    <div style="height: 140px;width: 100%;background-color: white;padding-top: 5px;">
      <textarea v-model="textarea" class="text-area"></textarea>
    </div>
    
    <div style="height: 40px;width: 95%;background-color: white;text-align: right;">
      <button v-on:click="getText()" class="btn-24"><span>发送</span></button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      textarea: "",
      messageList: [],
      id: 0
    }
  },
  watch: {

    'processdata': 'scrolltobottom'

  }
  ,
  methods: {

    scrolltobottom: function () {

      this.$nexttick(() => {

        var div = document.getelementbyid('data-list-content')

        div.scrolltop = div.scrollheight

      })

    },
    getText() {
      let nowText = this.textarea
      this.id = this.id + 1
      this.messageList.push({ message: nowText, position: "right", id: this.id })
      let messageList = this.messageList;

      this.id = this.id + 1
      let id = this.id
      function getResponse() {
        messageList.push({ message: "这真是一条好需求啊！", position: "left", id: id })
      }

      setTimeout(getResponse, 1000)
      console.log("tetxtt", this.textarea)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.text-area {
  width: 98%;
  height: 120px;
  border: solid 0px;
  outline: none;
  resize: none;
  border-radius: 10px;
}
</style>
