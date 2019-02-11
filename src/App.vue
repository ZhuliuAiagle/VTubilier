<template>
  <div id="app">
    <el-container>
      <div style="height: 60px;"></div>
      <!-- author-dialog -->
      <el-dialog width="30%" title="关于本项目的作者" :visible.sync="authorDialogVisible" center>
        <div class="author-dialog-content" style="text-align: center">
          <img src="./assets/neko.jpg" class="dialog-avatar"/>
          <h1>Chubby🐰</h1>
          <p>咸鱼大三天狗，喜欢VTuber，喜欢P社，喜欢旅游</p>
          <p>GitHub: ZhuliuAiagle</p>
          <p>Mail: hemmyshen@gmail.com</p>
          <p>Twitter: @hemmyshen</p>
        </div>
      </el-dialog>
      <!-- qrcode-dialog -->
      <el-dialog width="40%" title="您的支持是我不断创作的动力QAQ" :visible.sync="qrcodeDialogVisible" center>
        <div class="author-dialog-content" style="text-align: center">
          <img src="./assets/wechat.jpg" class="qrcode"/>
        </div>
      </el-dialog>
      <!-- page -->
      <el-header>
          <el-row>
              <el-col :span="1" style="font-size: 20px text-align: center">
                <el-dropdown @command="handleCommand">
                  <el-button type="primary">
                    <img src="./assets/sidebar.png" style="left: 0; width: 20px; vertical-align: middle;">
                  </el-button>
                  <el-dropdown-menu slot="dropdown"> 
                    <el-dropdown-item command="a">关于作者</el-dropdown-item>
                    <el-dropdown-item command="b">感谢作者</el-dropdown-item>
                  </el-dropdown-menu>
                </el-dropdown>
              </el-col>
              <el-col :span="2" style="font-size: 20px text-align: center">
                <img src="./assets/bili.png" style="width: 60px; vertical-align: middle;">
              </el-col>
              <el-col :span="16" style="text-align: left; font-size: 20px">
                VTuber粉丝数实时排行榜
              </el-col>
              <el-col :span="5" style="text-align: right;">
                  v0.5.4 by zijin
              </el-col>
          </el-row>
      </el-header>
      <el-main style="text-align:left;">
          <div style="line-height: 50px;">
            <h2 v-if="!loaded" style="vertical-align: middle; color: white;"><i class="el-icon-loading"></i>&nbsp;正在从bilibili API获取数据，请稍后...</h2>
            <h2 v-else style="vertical-align: middle; color: white;"><i class="el-icon-date"></i>&nbsp;截至{{date}}&nbsp;&nbsp;
            <el-button @click="refresh()" type="primary" icon="el-icon-refresh" style="vertical-align: middle" circle></el-button></h2>
          </div>
          <div id="fill" v-if="!loaded"></div>
          <card  v-for="(item, index) in items" :show="loaded" :key="index" :rank="index+1" :name="item[0]" :fans="item[1]" :belong="item[2]" :avatar="item[3]"></card>
      </el-main>
    </el-container>
  </div>
</template>

<script>
import Card from "./components/Card.vue";
import axios from 'axios';

export default {
  name: 'app',
  components: {
    Card
  },
  data:function(){
    return{
      items: [],
      date: "",
      loaded: false,
      reloaded: true,
      authorDialogVisible: false,
      qrcodeDialogVisible: false
    }
  },
  created: function(){
    var that = this
    axios.get("http://localhost:5000/fans")
    .then(function(response){
      console.log(response["data"]);
      that.items = response["data"];
      that.loaded = true;
      that.date = that.getDate()
    })
  },
  computed: {
    
  },
  methods: {
    refresh: function(){
      this.loaded = false;
      var that = this
      axios.get("http://localhost:5000/fans")
      .then(function(response){
        console.log(response["data"]);
        that.items = response["data"];
        that.loaded = true;
        that.date = that.getDate()
      })
    },
    getDate: function(){
      var date = new Date();
      return date.getFullYear().toString() +'-'+ (date.getMonth()+1).toString() + '-' + date.getDate().toString() + ' ' + 
      date.getHours().toString() + ':' + (date.getMinutes().toString().length < 2 ? "0":"") + date.getMinutes().toString() 
    },
    handleCommand: function(command){
        if(command == 'a'){
          this.authorDialogVisible = true;
        }else if(command == 'b'){
          this.qrcodeDialogVisible = true;
        }else{
          alert("正在开发中，敬请期待！")
        }
    }
  }
}
</script>

<style>
#app{
  background-image: url("./assets/fubuki.jpg");
  background-attachment: fixed;
}
.el-header{
    left: 0;
    right: 0;
    text-align: center;
    line-height: 60px;
    /*box-shadow: 3px 3px 7px #B3C0D1;*/
    z-index: 999;
    position: fixed;
    background-color: teal;
    color: white;
    opacity:0.9;
}
.el-aside{
    text-align: left;
    line-height: 400px;

}
.el-footer{
    background-color: #B3C0D1;
    color: #333;
    text-align: center;
    line-height: 60px;
}
.el-menu{
    background-color: #B3C0D1;
}
.el-column{
    text-align: center;
}
#fill{
  height: 500px;
}
.dialog-avatar{
    width: 150px;
}
.qrcode{
  width: 300px;
}
</style>
