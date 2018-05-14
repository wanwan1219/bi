<template>
    <div>
        <div class="search-title">
            <el-input v-model="courseName" placeholder="请输入课程名称" class="search"></el-input><el-button type="primary" @click="getmoudel(10)">查询</el-button>
        </div>
        <el-select v-model="status" placeholder="按照状态">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.value"
              :value="item.value">
            </el-option>
        </el-select>
        <el-date-picker
            v-model="startDate"
            type="date"
            value-format="yyyy-MM-dd"
            placeholder="选择开始日期">
        </el-date-picker>
        <el-date-picker
            v-model="endDate"
            type="date"
            value-format="yyyy-MM-dd"
            placeholder="选择结束日期">
        </el-date-picker>
        <el-button type="primary" @click="getmoudel(20)">搜索</el-button>
        <el-button type="primary" @click="newfile(1)">新建</el-button>
        <div class="file-wrap" v-if="fileStatus">
            <div class="new-file">
                <h2 class="ct">{{creat==true ? '添加问题' : '修改问题'}}</h2>
                <el-form label-width="80px">
                    <el-form-item label="问题标题:">
                        <el-input v-model="questionTitle" placeholder="标题5~30个汉字"></el-input>
                    </el-form-item>
                    <el-form-item label="问题内容:">
                        <el-input type="textarea" v-model="questionCotent" class="question-content"></el-input>
                    </el-form-item>
                    <div class="button-box">
                        <el-button type="primary" @click="close">取消</el-button>
                        <el-button type="primary" @click="creatFile">确定</el-button>
                    </div>
                </el-form>
            </div>
        </div>
        <div v-for="item in courseList">
            <div class="course">
                <span>{{item.courseId}}</span>
                <el-button type="primary" @click="newfile(2)">编辑</el-button>
            </div>
        </div>
        <el-button type="primary" @click="test">验证</el-button>
        <!-- <add v-if="showadd" :showadd="showadd" @closeadd="changeadd"></add> -->
        <add v-if="showadd" :showadd.sync="showadd"></add>
    </div> 
</template>

<script>
import add from '../add.vue'
export default {

  name: '',

  data () {
    return {
        courseName:'',
        status:'',
        options:[{
            value: '正常',
        }, {
            value: '被打回'
        }, {
            value: '审核中'
        }],
        startDate:'',
        endDate:'',
        fileStatus:false,
        questionTitle:'',
        questionCotent:'',
        courseList:[{'courseId':1},{'courseId':2},{'courseId':3}],
        creat:false,
        change:false,
        showadd:false,
    }
  },
  methods: {
    getmoudel (type) {
        if(type==10){
            this.status='';
            this.startDate='';
            this.endDate='';
        }else if(type==20){
            this.courseName='';
        }
        console.log(this.status,this.startDate,this.endDate,this.courseName)
    },
    newfile (type) {
        this.fileStatus=true;
        if(type == 1){
            this.creat=true;
            this.change=false;
        }else{
            this.change=true;
            this.creat=false;
        }
    },
    close () {
        this.fileStatus=false;
    },
    creatFile () {
        if(this.creat){
            console.log("创建")
        }else{
            console.log("修改")
            
        }
        this.fileStatus=false;
    },
    test () {
        this.showadd=true;
    },
    changeadd (val) {
        this.showadd= val;
    }
  }, 
  components: {add,} 
}
</script>

<style lang="scss" scoped>
/**{
    padding: 0;
    margin:0;
}*/
.fl{
    float: left;
}
.fr{
    float: right;
}
.clearfix:after {
    content: "";
    display: block;
    clear: both;
}
.clearfix {
    zoom: 1;
}
.ct{
    text-align: center;
}
.cu{
    cursor: pointer;
}
.search-title{
    padding-bottom: 10px;
    .search{
        width: 210px;
        margin-right: 5px;
    }
    
}
.file-wrap{
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.6);
    position: fixed;
    top:0;
    left: 0;
    .new-file{
        transform: translateY(15%);
        width: 90%;
        height: 80%;
        margin: auto;
        background: #fff;
        padding: 20px;
        .button-box{
            width: 100%;
            padding-top: 50px;
            text-align: center;
        }
    }
}
.course{
    width: 80%;
    margin: 20px auto;
    border: 1px solid #ccc;
    padding: 10px;
}

</style>
<style>
    .question-content textarea{
        height: 300px;
    }
</style>