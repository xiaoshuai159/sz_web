<template>
  <div class="app dp"
    v-loading="loading"
    element-loading-text="拼命加载中"
    element-loading-spinner="el-icon-loading"
    element-loading-background="rgba(0, 0, 0, 0.8)"
  >
    <!-- 头部 -->
    <div class="top1">
      <div class="top1_title">
        <!-- 头部_一级标头 -->
        <!-- 中卫市境外网址处置平台 -->
        <div class="title">深圳市反诈技术支撑平台</div>
        <!-- 7.4 测试暂时关闭 -->
        <!-- <div class="topguanli">
          <div class="role">
            <img :src="require('../assets/img/shouye/guanliyuan.png')" alt="" />
            
            <el-dropdown
              class="user-name"
              trigger="click"
              @command="handleCommand"
            >            
              <span class="el-dropdown-link">
                {{ username }}
                <i class="el-icon-caret-bottom"></i>
              </span>
              <template #dropdown>
                <el-dropdown-menu>
                  <el-dropdown-item command="upload">修改密码</el-dropdown-item>
                  <el-dropdown-item divided command="loginout"
                    >退出登录</el-dropdown-item
                  >
                </el-dropdown-menu>
              </template>
            </el-dropdown>
          </div>
        </div>
        <div>
        <div class="fanhui" @click="ret1">
          <img src="../assets/img/shouye/返回按钮.png" alt="" />
        </div>
      </div> -->
      </div>
    </div>
    <div class="nav">
      <div class="nav_li">
        <ul class="daohang3">
          <li
            v-for="(item, index) in daohang"
            :key="index"
          >
            <span>
              <router-link
                :to="{ path: item.path, query: { name: item.name } }" class="classA"
                >{{ item.name }}</router-link
              >
            </span>
          </li>
        </ul>
      </div>
    </div>
    <div class="center2">
      <div class="center2_left">
       <div class="gsh3"><span>·</span> 处置统计</div>
       <br>
       <div class="center2_left_test1">总处置数量</div>
       <div class="picture_test1"></div>
       <div class="picture_test2"></div>
       <div class="picture_test2"></div>
       <div class="picture_test2"></div>
       <div class="picture_test2"></div>
       <div class="picture_test2"></div>
       <div class="picture_test2"></div>
       <div class="center2_left_test2">昨日处置数量</div>
       <div class="picture_test3"></div>
       <div class="picture_test4"></div>
       <div class="picture_test4"></div>
       <div class="picture_test4"></div>
       <div class="center2_left_test3">日环比 12%</div>
       <img class="picture_test12" src="../assets/img/newbg/weixin3.png">
      </div>
      <div class="center2_center">
        <div class="gsh3"><span>·</span> 7日处置高危类型TOP5</div>
        <div ref="mypieChart" class="add1"></div> 
      </div>
      <div class="center2_right">
        <div class="gsh3"><span>·</span> 7日处置量趋势</div>
        <div style="width: 100%;height: 80%;">
          <div ref="mybarChart" style="width:100%; height: 100%;"></div>
        </div>
      </div>
    </div>

    <div class="center3">
      <div class="center3_one">
        <div class="gsh3"><span>·</span> 处置涉诈类型分析</div>
        <div class="pos">
          <input type="checkbox" class="box"><span class="pos1">全部类型</span><br>
          <input type="checkbox" class="box">虚假贷款<br>
          <input type="checkbox" class="box">虚假电商<br>
          <input type="checkbox" class="box">虚假理财<br>
          <input type="checkbox" class="box">杀猪盘赌<br>
          <input type="checkbox" class="box">下载链接<br>
          <input type="checkbox" class="box">冒充客服<br>
          <input type="checkbox" class="box">刷单返利<br>
          <input type="checkbox" class="box">网络游戏<br>
          <input type="checkbox" class="box">网络婚恋
        </div>
        <div style="display: inline-block; margin-left: 15px; width: 88%; height: 70%;" class="add2">
          <div ref="mylineChart" style="width:100%; height: 100%;"></div>
        </div>
      </div>
    </div>

    <div class="myfooter"></div>

    <el-dialog
      title="修改密码"
      :visible.sync="dialog"
      width="30%"
      :before-close="handleClose1"
      class="dialogInfo"
      :close-on-click-modal="false"
    >
      <el-form
        :rules="rules"
        ref="newdomainSimpleVo"
        label-width="80px"
        :inline="true"
        :model="newdomainSimpleVo"
        class="demo-form-inline search_select_form"
        size="mini"
      >
        <el-form-item label="原密码" prop="oldpwd">
          <el-input
            v-model="newdomainSimpleVo.oldpwd"
            placeholder="请输入原密码"
          ></el-input>
        </el-form-item>
        <el-form-item
          label="原密码"
          prop="oldpwd"
          style="visibility: hidden"
          v-if="yinc"
        >
          <el-input
            v-model="newdomainSimpleVo.oldpwd"
            placeholder="请输入原密码"
          ></el-input>
        </el-form-item>
        <el-form-item label="新密码" prop="xinpwd">
          <el-input
            v-model="newdomainSimpleVo.xinpwd"
            placeholder="请输入新密码"
          ></el-input>
        </el-form-item>
        <el-form-item label="确认密码" prop="xinpwd2">
          <el-input
            v-model="newdomainSimpleVo.xinpwd2"
            placeholder="请再次输入新密码"
          ></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button
          @click="submitForm('newdomainSimpleVo')"
          type="primary"
          size="mini"
          class="el-button-xitongerr"
          >修 改</el-button
        >
        <el-button
          class="el-button-xitongup"
          type="primary"
          @click="err"
          size="mini"
          >取 消</el-button
        >
      </span>
    </el-dialog>
  </div>
</template>

<script>
import dayjs from 'dayjs'

export default {
  // components: { getDiscover },
  data() {
    let pwd = JSON.parse(window.sessionStorage.getItem('pwd'))
    var validatePwd = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入原密码'))
      } else if (value !== pwd) {
        callback(new Error('原密码不正确!'))
      } else {
        callback()
      }
    }
    var validatePass2 = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请再次输入密码'))
      } else if (value !== this.newdomainSimpleVo.xinpwd) {
        callback(new Error('两次密码不一致!'))
      } else {
        callback()
      }
    }
    return {
      rules: {
        oldpwd: [
          {
            required: true,
            message: '密码不能为空',
            trigger: 'change',
          },
          {
            validator: validatePwd,
            trigger: 'blur',
            required: true,
          },
        ],
        xinpwd: [
          {
            required: true,
            message: '密码不能为空',
            trigger: 'blur',
          },
          { min: 8, max: 12, message: '密码长度为8到12位', trigger: 'blur' },
          {
            pattern: /^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{8,16}$/,
            message: '格式不正确，请输入数字与字母组合',
            trigger: 'blur',
          },
        ],

        xinpwd2: [
          {
            required: true,
            message: '确认密码不能为空',
            trigger: 'change',
          },
          {
            validator: validatePass2,
            trigger: 'blur',
            required: true,
          },
        ],
      },
      //居中导航
      daohang: [
        // 7.4 新增测试数据
      {
          name: '首页',
          path: '/shouye',
        },{
          name: '发现',
          path: '/gitDiscoverca',
        },{
          name: '处置'
        },{
          name: '警情',
          path:'/getWarningfourg'
        },{
          name: '特办'
        },{
          name: '关联'
        },
        {
          name: '管理',
          path: '/findUser',
        },
      ],
      daohang1: [
        {
          name: '首页',
          path: '/shouye',
        },{
          name: '发现',
          path: '/gitDiscoverca',
        },{
          name: '预警',
          path:'/getWarningfourg'
        },
        {
          name: '系统管理',
          path: '/findUser',
        },
      ],
      daohang2: [
      {
          name: '首页',
          path: '/shouye',
        },
           {
          name: '预警',
          path:'/getWarningfourg'
        },
        {
          name: '发现',
          path: '/gitDiscoverca',
        },
      ],
      //头部loading
      // loading: true, // 7.4 测试暂时关闭
      loading: false,
      pid: [],
      Newname: [],
      Newname1: [],
      active: '',
      items: [],
      name: 'wu',
      dialog: false,
      newdomainSimpleVo: {
        oldpwd: '',
        xinpwd: '',
        xinpwd2: '',
      },
      yinc: false,
      // loading1: false,
    }
  },
  computed: {
    // username() {
    //   let username = JSON.parse(window.sessionStorage.getItem('one'))
    //   return username ? username : this.name
    // },
  },
  mounted() {
    this.drawpieChart()
    this.drawbarChart()
    this.drawlineChart()
  },
  methods: {
    dh() {
      let dntit = JSON.parse(window.sessionStorage.getItem('qx'))

      let cj = 'CD_SUPER_ADMIN'

      if (dntit == cj) {
        this.daohang = this.daohang1
      } else {
        this.daohang = this.daohang2
      }
    },
    //导航

    handleClose1() {
      this.dialog = false
    },
    handleCommand(command) {
      if (command == 'loginout') {
        // localStorage.removeItem("ms_username");
        this.tuichusession()
        window.sessionStorage.clear()
        this.$router.push('/home')
      } else if (command == 'upload') {
        this.dialog = true
      }
    },
    async tuichusession() {
      const { data: res } = await this.$http.post('/pagelogout')
    },
    submitForm(formName) {
      let that = this
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.up()
          // this.dialogVisible = false
          this.$nextTick(() => {
            that.$refs['newdomainSimpleVo'].clearValidate()
          })
        } else {
          // console.log('error submit!!');
          return false
        }
      })
    },
    async up() {
      let list = {
        oldPassword: this.newdomainSimpleVo.oldpwd,
        newPassword: this.newdomainSimpleVo.xinpwd,
        newPassword1: this.newdomainSimpleVo.xinpwd2,
      }
      const { data: res } = await this.$http.post('/user/modifyPwd', list)
      if (res.code == 200) {
        this.dialog = false
        this.$message(res.data)
        this.$router.push('/home')
      } else {
        this.$message(res.data)
        this.dialog = false
      }
    },
    err() {
      this.newdomainSimpleVo.oldpwd = ''
      this.newdomainSimpleVo.xinpwd = ''
      this.newdomainSimpleVo.xinpwd2 = ''
      let that = this
      this.$nextTick(() => {
        that.$refs['newdomainSimpleVo'].clearValidate()
      })
      this.dialog = false
    },
    drawpieChart() {
      let bar_qx = this.$refs.mypieChart
      let pieChart = this.$echarts.init(bar_qx)
      window.addEventListener('resize', function () {
        pieChart.resize()
      })
      pieChart.clear()
      pieChart.setOption(this.setOptionPie()) // 待完善：记得销毁echarts和resize
    },
    setOptionPie() {

      let option = {
        toolbox: {
          show: true
        },

        series: [
          {
            name: 'Nightingale Chart',
            type: 'pie',
            radius: [10, 90],
            center: ['50%', '50%'],                                        
            roseType: 'area',                                                     
            itemStyle: { 
              borderRadius: 8
            },                                                
            data: [
              { value: 26, name: '40%赌博',itemStyle: {color:'red'}},
              { value: 25, name: '21%刷单返利',itemStyle: {color:'green'}},
              { value: 20, name: '15%理财',itemStyle: {color:'blue'}},
              { value: 15, name: '14%贷款',itemStyle: {color:'purple'}},
              { value: 10, name: '8%网络婚恋，交友',itemStyle: {color:'pink'}}
            ]
          }
        ]
      }
      // this.loading1=false
      return option
    },

    drawbarChart() {
      let bar_qx = this.$refs.mybarChart
      let barChart = this.$echarts.init(bar_qx)
      window.addEventListener('resize', function () {
        barChart.resize()
      })
      barChart.clear()
      barChart.setOption(this.setOptionBar()) // 待完善：记得销毁echarts和resize
    },
    setOptionBar() {


      let option = {

        title: {
          text: ''
        },


        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'shadow'
          }
        },


        legend: 
        {
          x:'right',
          y:'top',
          padding:[10,50,0,0],
          orient: 'vertical',
          textStyle:
          {
            color:'#ccc',
          }
        },

        grid: {
          left: '5%',
          right: '6%',
          bottom: '10%',
          containLabel: true
        },


        xAxis: {   
          type: 'category',
          data: ['5.17', '5.18', '5.19', '5.20', '5.21', '5.22','5.23'],
          axisLine: 
          {  
                lineStyle: {
                    color: "#fff",
                }
          },
          splitLine:
          {
          show:false
          }
        },


        yAxis: 
        {  
        splitLine:
        {
          show:false
        },
          type: 'value',
          interval:100, 
          min:0, 
          max:500,
          axisLine: 
          {  
                lineStyle: {
                    color: "#fff",
                }
          }
        },


        series: [
          {
            name: '同源',
            type: 'bar',
            data: [283, 346, 251, 412, 141, 312,415],
            itemStyle:
            {
                normal:
              {
                color:'#4ad2ff'
              }
            },
          },
          
          {
            name: '警情',
            type: 'bar',
            data: [165,252,324,163,245,451,255]
          },
        ]


      }
      // this.loading1=false
      return option
    },

    drawlineChart() {
      let line_qx = this.$refs.mylineChart
      let lineChart = this.$echarts.init(line_qx)
      window.addEventListener('resize', function () {
        lineChart.resize()
      })
      lineChart.clear()
      lineChart.setOption(this.setOptionLine()) // 待完善：记得销毁echarts和resize
    },
    setOptionLine(){
      let option = {

     

        xAxis: {
          axisLine: {
            lineStyle: {
              color: '#912CEE'
            }
          },
          type: 'category',
          boundaryGap: true,
          axisLabel: {
            interval: 0  
            },
          data: ['1', '2', '3', '4', '5', '6', '7','8','9','10','11', '12', '13', '14', '15', '16', '17','18','19','20','21', '22', '23', '24', '25', '26', '27','28','29','30']
        },

        grid: {
            top: '16%',   
            left: '3%', 
            right: '8%',
            bottom: '3%',
            containLabel: true
        },

        yAxis: {
          type: 'value',
          name: '星期/月',
          interval:500, 
          min:0, 
          max:3500 ,
          axisLine: {
            lineStyle: {
              color: '#87CEFA'
            }
          },
        },

        legend: 
        {
          x:'right',
          y:'top',
          padding:[2,300,200,100],
          orient: 'horizontal',
          textStyle:
          {
            color:'#ccc',
          }
        },

        series: [
          {
            name:'同源处理量',
            data: [1000,2423,2357,2315,1257,2351,3123,1953,2584,1954,2334,2751,2407,3256,1169,2536,2451,2345,2238,3128,3321,1260,2475,2453,1112,2765,1238,2431,1230,2351],
            type: 'line',
            areaStyle: {},
            symbol:'square',
            symbolSize:5,
            itemStyle:{
            borderColor:'purple',
            normal: 
            {
            color: 'purple', 
            lineStyle: 
            {
              color: 'purple' 
            }
            }
            }
          },

          {
            name:'案件量',
            data: [1400,2523,2257,2515,3257,1351,3143,1653,2484,1254,2634,2751,2607,3251,1269,3136,1451,3345,1238,3128,1321,2260,3475,1453,3112,1765,3238,1431,3230,1351],
            type: 'line',
            areaStyle: {},
            symbol:'square',
            symbolSize:5,
            itemStyle:
            {
              borderColor:'yellow',
              normal: 
              {
            color: 'yellow', 
            lineStyle: 
            {
            color: 'yellow' 
            }
              }
            }
          }

        ]
      }
      // this.loading1=false
      return option
    },

    ret1() {
      this.$router.push('/home')
      window.localStorage.clear()
      window.sessionStorage.clear()
    },
  },
}
</script>

<style  scoped lang='less'>
@import '../common/font.css';

.add1{
  height: 300px;
  width: 500px;
  margin-left: 50px;
  margin-bottom: 200px;
  margin-right: 500px;
}
.dp {
  width: 100%;
  height: 100%;
  /* width: 100%;
  height: 58.5625rem /* 937/16 */
  /* // z-index: -1; */
  background-color: #020723;
  // background-image: url('../assets/newimg/newhome/背景.png');
  // background-repeat: no-repeat;
  // background-size: 100% 100%;
}
.top1 {
  width: 100%;
  height: 9.7%;
  /* background-color: blue; */
  /* border: 1px solid blue; */
  position: relative;
}
.title {
  // position: absolute;
  // left: 38%;
  // top: 15%;
  text-align: center;
  font-family: 'heiti';
  // font-size: 31px;
  height:75px;
  line-height: 75px;
  font-size: 42px;
  letter-spacing: 7px /* 12/16 */;
  color:#19c9d5;
  // 实现文字渐变 background-image、-webkit-background-clip、-webkit-text-fill-color
  // background-image: linear-gradient(0deg, #f8f8fb 0%, #8ec4fd 100%);
  // -webkit-background-clip: text;
  // -webkit-text-fill-color: transparent;
  // text-shadow: 0 0 20px rgba(0, 121, 255, 0.5);
}
.top1_title {
  height: 100px;
  width: 100%;
  background-image: url('../assets/img/newbg/header_bg.png');
  background-repeat: no-repeat;
  // background-size: cover;  // 不会让图片走形，尽可能完整
  background-size: 100% 100%; // 根据容器大小拉伸图片，会让图片走形
}
.top1_line {
  height: 10px;
  width: 100%;
  // background: url('../assets/newimg/newhome/光.png') no-repeat;
  // background-size: 100% 100%;
}
.center2 {
  display: flex;
  justify-content: space-between;
  margin: 0 36px;
}

.gsh3{
  margin-top: 28px;
  margin-left: 28px;
  font-size: 20px;
}

.center2_left,.center2_center,.center2_right{
  font-size: 20px;
  color:#32c5ff;
  height: 360px; // width/height=1.58
  width: 600px;
  margin-top: 20px;
  margin-left: 20px;
  margin-right: 30px;
  background: url(../assets/img/newbg/kuangbg.png);
  background-size: 100% 100%;
  background-repeat: no-repeat;
}

.center2_left_test1{
  color: #72e7ee;
  margin-left: 38px;
}
.center2_left_test2{
  color:#c9c391;
  margin-top: 14px;
  margin-left: 38px;
}
.center2_left_test3{
  display: inline-block;
  // margin-left: 30px;
  color: #18c5e7;
  // vertical-align:super;
  transform: translate(40px,-20px);
  font-size:20px;
}
.picture_test1{
  display: inline-block;
  background-image: url(../assets/img/newbg/lanbg.png);
  background-size: 100% 100%;
  width: 52px;
  height: 52px;
  margin-left: 68px;
  margin-top: 20px;
}
.picture_test2{
  display: inline-block;
  background-image: url(../assets/img/newbg/lanbg.png);
  background-size: 100% 100%;
  width: 52px;
  height: 52px;
  margin-left: 13px;
  margin-top: 20px;
}
.picture_test3{
  display: inline-block;
  background-image: url(../assets/img/newbg/weixin2.png);
  background-size: 100% 100%;
  width: 52px;
  height: 52px;
  margin-left: 68px;
  margin-top: 20px;
}
.picture_test4{
  display: inline-block;
  background-image: url(../assets/img/newbg/weixin2.png);
  background-size: 100% 100%;
  width: 52px;
  height: 52px;
  margin-left: 13px;
  margin-top: 20px;
}
.picture_test12{
  vertical-align:text-top;
  width: 14px;
  transform: translate(50px,-16px);
}

.pos{
  display: inline-block;
  margin-left: 90px;
  font-size: 19px;
  color:#fff;
}
.pos1{
  color:#72e7ee;
  margin-top: 7px;
}
.box{
  border-style: solid;
  width: 21px;
  height: 11px;
  border: 2px solid blue;
  outline: none;
}
.center3{
  margin-top: 30px;
  margin-left: 55px;
  margin-right:65px
}
.center3_one{
  font-size: 17px;
  height: 356px;
  width: 100%;
  background: url(../assets/img/newbg/kuangbg2.png);
  background-size: 100% 100%;
  background-repeat: no-repeat;
  color:#32c5ff;
  overflow: hidden;
}
.center1 {
  width: 100%;
  height: 91.3%;
}
.center1_left,
.center1_map,
.center1_center,
.center1_right {
  float: left;
  display: flex;
  flex-flow: column;
}
.center1_left {
  width: 448px;
  height: 100%;
  padding-left: 28px;
  padding-top: 38px;
  box-sizing: border-box;
  // border: 1px solid red;
}


.nav {
  width: 1027px;
  height: 3.2%;
  margin: 0 auto;
  // border: 1px solid red;
  // background: url('../assets/newimg/newhome/形状 6.png') no-repeat;
  // background-size: 100% 100%;
  position: relative;
  // margin-top: 15px;
  // border: 1px solid #ccc;
}
.nav_li {
  // width: 900px;
  height: 100%;
  // border: 1px solid blue;
  // margin: 0 120px;
  // margin-left: 60px;
  // padding-top: 10px;
  box-sizing: border-box;
  float: left;
  position: relative;
  left: 50%;
  // overflow: hidden;
}
.classA{
  color:rgba(0, 255, 243, 0.75);
  font-size: 20px 
}
.classA:hover{
  color: rgba(0, 255, 243);
}
.daohang3 {
  // margin: 0 auto;
  // display: table;
  position: relative;
  left: -50%;
}
.nav_li .daohang3 li {
  // display: inline-block;
  font-family: '黑体';
  float: left;
  list-style-type: none;
  text-align: center;
  height: 40px;
  line-height: 40px;
  width: 100px;
  margin-right: 8px;
  background: url(../assets/img/newbg/btnbg.png);
  background-size: cover;
  color:#00fff3
  // box-shadow: 0 0 10px #1768be inset, 0 0 0px #b5c5d4;
}
.nav_li .daohang3 li span{
  color:#00fff3
}

.nav_li .daohang3 li:hover {
  // background-image: linear-gradient(0deg, #0a35f5 0%, #07f8e4 100%);
  // -webkit-background-clip: text;
  // -webkit-text-fill-color: transparent;
  color: #fff;
  border-bottom-color: #07e7e6;
  //  text-shadow: 0 0 20px rgba(189, 207, 228, 0.5);
}

.myfooter {
  position:fixed;
  bottom: 8px;
  height: 6vh;
  width: 100%;
  background: url(../assets/img/newbg/bottombg.png);
  background-size: 100% 100%;
  background-repeat: no-repeat;
}
.fanhui {
  position: absolute;
  left: 0.5%;
  top: 15%;
  cursor: pointer;
}
.fanhui img {
  width: 75px;
  height: 40px;
  // background-color: rgb(209, 77, 94);
}
.topguanli {
  width: 8.1% /* 150/16 */ /* 277/16 */;
  height: 50% /* 30/16 */ /* 35/16 */;
  position: absolute;
  right: 0%;
  top: 20%;
}
.role {
  height: 30px /* 30/16 */;
}
.role img {
  height: 30px;
  vertical-align: middle;
  /* margin-top:10px */
}
.el-dropdown-link {
  color: #fff;
  cursor: pointer;
}
.user-name {
  margin-left: 10px;
}
.user-avator {
  margin-left: 20px;
}
.user-avator img {
  display: block;
  width: 40px;
  height: 40px;
  border-radius: 50%;
}
.dialogInfo /deep/ .el-dialog {
  background: #021c2d url(../assets/img/shouye/背景框.png) no-repeat;
  background-size: 100% 100%;
  padding: 10px;
  opacity: 0.85;
  box-sizing: border-box;
  .el-dialog__headerbtn {
    top: 0px !important;
    right: 6px !important;
    .el-dialog__close {
      color: #fff;
      font-size: 14px;
    }
  }
  .el-dialog__header {
    margin: 20px 50px 0px 40px;
    background: url(../assets/img/shouye/标题矩形.png) no-repeat;
    background-size: 100% 100%;
  }
  .el-dialog__title,
  .gailan h3,
  .gailan1 h3 {
    color: #2fbcfc;
  }
  .gailan h3,
  .gailan1 h3 {
    padding-left: 10px;
  }
}
.dialog-footer /deep/ .el-button-xitongerr:focus,
.dialog-footer /deep/ .el-button-xitongerr:hover {
  background: url(../assets/img/shouye/确定按钮.png) no-repeat;
  background-size: cover;
  border: none;
  color: #25c0fd;
}
.dialog-footer /deep/ .el-button-xitongup {
  background: url(../assets/img/shouye/取消按钮.png) no-repeat;
  background-size: cover;
  border: none;
}
.dialog-footer /deep/ .el-button-xitongerr {
  background: url(../assets/img/shouye/确定按钮.png) no-repeat;
  background-size: cover;
  border: none;
  color: #25c0fd;
}
.bg {
  color: rgb(118, 226, 255);
}
</style>