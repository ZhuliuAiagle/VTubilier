<template>
  <div id="app">
    <el-container>
      <div style="height: 60px;"></div>
      <el-header>
          <el-row>
              <el-col :span="1" style="font-size: 20px text-align: center">
                  <img src="./assets/sidebar.png" style="left: 0; width: 20px; vertical-align: middle;">
              </el-col>
              <el-col :span="2" style="font-size: 20px text-align: center">
                <img src="./assets/bili.png" style="width: 60px; vertical-align: middle;">
              </el-col>
              <el-col :span="16" style="text-align: left; font-size: 20px">
                VTuber粉丝数实时排行榜
              </el-col>
              <el-col :span="5" style="text-align: right;">
                  v0.5.2 by zijin
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
      reloaded: true
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
    }
  }
}
</script>

<style>
#app{
  background-image: url("./assets/fubuki.jpg");
  background-repeat: no-repeat;
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
</style>
