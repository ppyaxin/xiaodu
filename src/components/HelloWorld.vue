<template>
  <div style="position: relative;" @click="cancelJudge($event)">
    <!-- <div style="position: absolute;width: 50px;height: 25px;background-color: red;left:0"
      v-bind:style="{ top: leftY + 'px' }">
      kkk
    </div> -->
    <div ref="QAContent"
      style="height: 400px;width: 100%;overflow-x: hidden;overflow-y: auto; background-color: aliceblue;border-radius: 10px;">
      <div>
        <div v-for="item in messageList" :key="item.id">
          <!-- 左侧 -->
          <div @contextmenu.prevent="rightClickText(item)" v-if="item.position == 'left'"
            style="width: 100%;display: flex;padding: 10px;">
            <div style="padding-right:5px">
              <img src="../assets/xiaodu.png" style="width: 50px;height: 50px;border-radius: 100%;">
            </div>

            <div>
              <div>
                <span style="font-size: 6px;color: #798588;">小杜小杜</span>
              </div>
              <div style="display: flex">
                <div style="background-color:#0E4A82;padding: 4px;border-radius: 5px;color: white;">
                  <span>{{ item.message }}</span>
                </div>
                <div v-if="item.good" style="padding-left: 4px;">
                  <img src="../assets/good.png" style="width: 24px;">
                </div>
                <div v-if="item.bad" style="padding-left: 4px;">
                  <img src="../assets/bad.png" style="width: 24px;">
                </div>
              </div>
              <div v-if="item.rightmouse" style="padding-top: 2px;">
                <button v-on:click="setGood(item)">{{ item.zan }}</button>
                <button v-on:click="setBad(item)">{{ item.cai }}</button>
              </div>
            </div>

          </div>
          <!-- 右侧 -->
          <div v-if="item.position == 'right'"
            style="width: 100%;display: flex;margin-right: 10px;flex-direction:row-reverse;padding-top: 30px;">
            <div style="padding-right:10px;padding-left: 5px;">
              <img src="../assets/user.jpg" style="width: 50px;height: 50px;border-radius: 100%;">
            </div>

            <!-- 右侧 -->
            <div style="width: 60%;">
              <div style="text-align: right;">
                <span style="font-size: 6px;color: #798588;">用户</span>
              </div>
              <div style="display: flex;flex-direction:row-reverse;word-break: break-all;">
                <div style="background-color: #81BC2B;padding: 4px;border-radius: 5px;">
                  <span>{{ item.message }}</span>
                </div>
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
      id: 0,

    }
  },
  watch: {

    'processdata': 'scrolltobottom'

  }
  ,
  created() {
    var that = this
    document.onkeydown = function (e) {
      var key = window.event.keyCode
      if (key == 13) {
        that.getText()
      }
    }
  },
  mounted() {
      this.scrollToBottom()
    },
  methods: {

    
    updated() {
    },
    getRandomMsg(){
      let index =  Math.floor(Math.random()*10); 
      let arr=["这真是一条好需求啊！","这真是一条不咋地需求啊！","这条需求感觉一般般","这条需求感觉一般般","这条需求感觉一般般","这条需求感觉一般般","这条需求感觉一般般","这条需求感觉一般般","这条需求感觉一般般","这条需求感觉一般般"]
      return arr[index]
    },
    getText() {
      if(this.textarea.length==0) return
      let nowText = this.textarea
      this.id = this.id + 1
      this.messageList.push({ message: nowText, position: "right", id: this.id })
      this.scrollToBottom()
      let messageList = this.messageList;
      this.id = this.id + 1
      let id = this.id
      let that = this
      function getResponse() {
        messageList.push({
          message: that.getRandomMsg(), position: "left", id: id, rightmouse: false, zan: "赞", cai: "踩", good: false,
          bad: false,
        })
        that.scrollToBottom()
      }
      setTimeout(getResponse, 1000)
      console.log("tetxtt", this.textarea)
      this.textarea=""
    },
    scrollToBottom() {
      this.$nextTick(() => {
        let scrollElem = this.$refs.QAContent
        console.log("scrollheight", scrollElem.scrollHeight)
        scrollElem.scrollTo(0, scrollElem.scrollHeight)
      })
    },
    rightClickText(item) {
      console.log("item", item)
      item.rightmouse = true
    },
    setGood(item) {
      if (item.good) {
        item.good = false
        item.zan = "赞"
      } else {
        item.good = true
        item.bad = false
        item.zan = "取消赞"
        item.cai = "踩"
      }
      item.rightmouse = false
    },
    setBad(item) {
      if (item.bad) {
        item.bad = false
        item.cai = "踩"
      } else {
        item.bad = true
        item.good = false
        item.cai = "取消踩"
        item.zan = "赞"
      }
      item.rightmouse = false
    },
    cancelJudge(e) {
      console.log("canceljudge")
      for (let i = 0; i < this.messageList.length; i++) {
        if (this.messageList.rightmouse) {
          this.messageList.rightmouse = false
        }
      }
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
  border-radius: 20px;
  font-size: larger
}

::-webkit-scrollbar {
  width: 10px;
  height: 10px;
}

::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background-color: 	#DCDCDC;
}

::-webkit-scrollbar-track {
  border-radius: 10px;
  background-color: 	#F5F5F5;
}
</style>
