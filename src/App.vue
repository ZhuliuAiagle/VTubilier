<template>
  <div id="app">
    <el-container>
      <div id="top" style="height: 60px;" ></div>
      <!-- author-dialog -->
      <el-dialog width="30%" title="About Me" :visible.sync="authorDialogVisible" center>
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
      <!--timeout-dialog-->
      <el-dialog width="50%" title="网络连接异常" :visible.sync="timeoutDialogVisible" center>
        <div class="author-dialog-content" style="text-align: center">
          <img src="./assets/timeout.png" class="timeout-icon"/>
          <h1>500: 服务器错误</h1>
          <p>请稍后尝试刷新页面，或检查网络连接</p>
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
                <a href="https://www.bilibili.com">
                  <img src="./assets/bili.png" style="width: 60px; vertical-align: middle;">
                </a>
              </el-col>
              <el-col :span="10" style="text-align: left; font-size: 20px">
                hololive个人用斗虫楼
              </el-col>
              <el-col :span="7">
                <el-input v-model="searchInput" placeholder="搜索Vtuber..."></el-input>
              </el-col>
              <el-col :span="4" style="text-align: right;">
                <a href="https://github.com/ZhuliuAiagle/VTubilier" style="color: white; text-decoration: none;">
                <el-popover
                placement="bottom"
                title="当前版本"
                width="200"
                trigger="hover" 
                content="点击访问Github仓库">
                  <span slot="reference">
                    v0.7.9 by Chubby
                  </span>
                </el-popover>
                </a>
              </el-col>
          </el-row>
      </el-header>
      <el-main style="text-align:left;">
          <div style="line-height: 50px;">
            <h2 v-if="!loaded" style="vertical-align: middle;">
              <i class="el-icon-loading"></i>&nbsp;正在从bilibili API获取数据，请稍候...</h2>
            <h2 v-else style="vertical-align: middle;"><i class="el-icon-date"></i>&nbsp;截至{{date}}&nbsp;&nbsp;
            <el-button @click="refresh()" type="primary" icon="el-icon-refresh" style="vertical-align: middle" circle></el-button>
            &nbsp;&nbsp;&nbsp;<el-checkbox v-model="checked">正在直播的账号</el-checkbox></h2>
          </div>
          <div id="fill" v-if="!loaded"></div>
          <card  v-for="(item, index) in items" :show="loaded" :key="index" :rank="index+1" :name="item[0]" :fans="item[1]" 
          :belong="item[2]" :avatar="item[3]" :space="item[4]" :living="item[5]==1?true:false" :display="checked" :ninki="item[6]" :title="item[7]"></card>
          <div id="fill" v-if="checked"></div>
      </el-main>
      <el-footer>
        <a href="#top">
            <el-button class="topback" type="primary" icon="el-icon-caret-top" 
            circle></el-button>
        </a>
      </el-footer>
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
      qrcodeDialogVisible: false,
      timeoutDialogVisible: false,
      timeoutMsg:"",
      searchInput: '',
      checked: false
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
    }).catch(function(error){
      that.timeoutMsg = error
      that.timeoutDialogVisible = true
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
  font-family:"Microsoft YaHei";
}
.el-header{
    left: 0;
    right: 0;
    text-align: center;
    line-height: 60px;
    z-index: 999;
    position: fixed;
    background-color: #409EFF;
    color: white;
    opacity:0.9;
}
.el-input__inner{
  height: 35px !important;
  opacity: 0.7 !important;
}
.el-checkbox__inner{
  zoom: 200%;
}
.el-checkbox__input.is-checked + .el-checkbox__label{
  font-weight: 800 !important;
}
.el-checkbox__label{
}
.el-checkbox{
  font-size: 20px !important;
}
.el-footer{
  left: 0;
  right: 0;
  bottom: 50px;
  position: fixed;
  line-height: 90px;
  z-index: 999;
  background-color:transparent;
  color: white;
  text-align:right;
}
.topback{
  bottom:20px;
  vertical-align: middle;
  font-size:30px !important;
  padding: 15px !important;
}
.el-aside{
    text-align: left;
    line-height: 400px;

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
.timeout-icon{
  width:100px;
}
</style>
