<template>
    <transition v-if="show&&(!display || living)" name="el-fade-in-linear">
        <div>
            <el-card :class="[living?'online-card':'offline-card']">
                <a :href="space" style="color:black;" target="_blank">
                    <el-row style="top: 0;">
                        <el-col span="4">
                            <div class="avatar-container">
                                    <img class="avatar" :src="avatar"/>
                            </div>
                        </el-col>
                        <el-col span="12" style="text-align: left; vertical-align: middle;">
                        <el-row>
                            <h1>{{rank}}. {{name}}<span v-if="living">(直播中)</span></h1>
                        </el-row>
                        <el-row style="display:none;">
                            <p>所属: {{belong}}</p>
                        </el-row>
                        </el-col>
                        <el-col span="8" style="text-align: right; vertical-align: middle;">
                        <p style="font-size: 23px;">{{fans}} 人</p>
                        </el-col>
                    </el-row>
                </a>
            </el-card>
            <br/>
        </div>
    </transition>
</template>

<script>
export default {
    name: "Card",
    props:{
        rank: Number,
        name: String,
        belong: String,
        fans: Number,
        avatar: String,
        show: Boolean,
        space: String,
        living: Boolean,
        display: Boolean
    }
    
}
</script>

<style>
.offline-card{
    opacity:0.9;
}
.online-card{
    opacity:0.9;
    background-color: #67C23A !important;
    border: 0px !important;
}
.online-card h1{
    color: white !important;
}
.online-card p{
    color: white !important;
}
.avatar{
    position:absolute;
    left:0px;
    top:0px; 
    border-radius:50%;
    -webkit-border-radius:50%;
    -moz-border-radius:50%; 
    width:60%;
    z-Index:-1;
}
.avatar-container{
    width:65%; 
    padding-top:10.9%; 
    margin-right:2.8%; 
    border-radius:50%;
    -webkit-border-radius:50%;
    -moz-border-radius:50%; 
    position:relative;
}
</style>


