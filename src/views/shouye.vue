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
      <div class="center2_left">处置统计
      </div>
      <div class="center2_center">7日处置高危类型TOP5
      </div>
      <div class="center2_right">7日处置量趋势
      </div>
    </div>

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
import 'echarts-gl'
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
    //左侧——趋势
    drawLinezhexian() {
      // this.loading1=true
      // eslint-disable-next-line camelcase
      var bar_qx = this.$refs.echartsshouye3zhexian
      // eslint-disable-next-line camelcase

      let myChart1 = this.$echarts.init(bar_qx)

      window.addEventListener('resize', function () {
        myChart1.resize()
        // console.log(1)
      })
      myChart1.clear()
      myChart1.setOption(this.setOptionzhexian())

      // console.log(this.qutest);
    },
    setOptionzhexian() {
      let option = {
        animation: false,
        feature: {
          saveAsImage: {
            show: false,
          },
        },

        tooltip: {
          trigger: 'axis',

          axisPointer: {
            lineStyle: {
              color: '#66B3FF',
            },
          },
        },
        color: ['#fc7278', '#fba64f', '#99f8f7'], //高 中 低
        legend: {
          right: '5%',
          top: '3%',
          icon: 'rect',
          itemHeight: 5,
          itemWidth: 10,
          data: [
            {
              name: '高危',
              textStyle: {
                color: ['#f9fefc'],
              },
            },
            {
              name: '中危',
              textStyle: {
                color: ['#f9fefc'],
              },
            },
            {
              name: '低危',
              textStyle: {
                color: ['#f9fefc'],
              },
            },
          ],
        },

        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: this.yujingqs.datanum.reverse(),
          axisLabel: {
            // rotate: -20,
            //  让x轴文字方向为竖向
            // interval: 0,
            //修改x轴文字
            textStyle: {
              color: '#f7ffff',
            },
          },

          axisLine: {
            lineStyle: {
              color: '#666',
              width: 1,
            },
          },
        },

        yAxis: [
          {
            type: 'value',
            axisLine: { show: false },
            axisTick: { show: false },
            splitLine: { show: false },
            splitLine: {
              lineStyle: {
                color: ['#1e324a'],
              },
            },
            axisLabel: {
              textStyle: {
                color: '#828d9a',
              },
            },
          },
        ],

        series: [
          {
            name: '高危',
            type: 'line',
            data: this.yujingqs.gaoweinum.reverse(),
            // smooth: true,
          },
          {
            name: '中危',
            type: 'line',

            data: this.yujingqs.inTotalnum.reverse(),
            // smooth: true,
          },
          {
            name: '低危',
            type: 'line',

            data: this.yujingqs.lowTotalnum.reverse(),
            // smooth: true,
          },
        ],

        grid: {
          x: 40,
          y: 40,
          x2: 20,
          y2: 20,
          borderWidth: 1,
        },
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
  height: 11%;
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
  height:80px;
  line-height: 80px;
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
  height: 110px;
  width: 100%;
  background: url('../assets/img/newbg/header_bg.png') no-repeat 100% 100%;
  // background-position: 0;
  background-size: cover;
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
}
.center2_left,.center2_center,.center2_right{
  height: 100px;
  margin: 10px;
  background-color: pink !important;
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
  height: 6%;
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