<template>
    <div>
        <el-card class="box-card">
            <el-row style="top: 0;">
                <el-col span="4">
                    <div class="avatar-container">
                        <img class="avatar" :src="avatar"/>
                    </div>
                </el-col>
                <el-col span="12" style="text-align: left; vertical-align: middle;">
                <el-row>
                    <h1>{{rank}}. {{name}}</h1>
                </el-row>
                <el-row>
                    <p>所属: {{belong}}</p>
                </el-row>
                </el-col>
                <el-col span="8" style="text-align: right; vertical-align: middle;">
                <p style="font-size: 40px; color: grey">{{fans}} 人</p>
                </el-col>
            </el-row>
        </el-card>
        <br/>
    </div>
</template>

<script>
export default {
    name: "Card",
    props:{
        rank: Number,
        name: String,
        belong: String,
        fans: Number,
        avatar: String
    }
}
</script>

<style>
.el-card{
    opacity:0.9;
}
.avatar{
    position:absolute;
    left:0px;
    top:0px; 
    border-radius:50%;
    -webkit-border-radius:50%;
    -moz-border-radius:50%; 
    width:100%;
    z-Index:-1;
}
.avatar-container{
    width:130px; 
    padding-top:10.9%; 
    margin-right:2.8%; 
    border-radius:50%;
    -webkit-border-radius:50%;
    -moz-border-radius:50%; 
    position:relative;
}
</style>
