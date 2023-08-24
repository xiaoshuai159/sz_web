<template>
  <div class="app dp">
    <div class="top1">
      <div class="top1_title">
        <div class="title">深圳市反诈技术支撑平台</div>
        <div class="nav" v-show="$route.path==='/findUser'||$route.path==='/findRole'||$route.path==='/dept'">       
        <el-button
          size="mini"
          type="primary"
          class="navnum"
          v-for="(item, index) in daohangnum"
          :key="index"
          :class="['/' + item.name !== $route.path ? '' : 'yangshi']"
          @click="navclick(item.name, item.menuName)"
          >{{ item.menuName }}</el-button
        >
      </div>
        <div class="topguanli">
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
        <div class="fanhui">
         {{curDate}}
        </div>
      </div>
      <!-- <div class="nav">
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
      </div> -->
    </div>
        <el-dialog
          title="修改密码"
          :visible.sync="dialog"
          width="30%"
          :before-close="handleClose1"
          class="dialogInfo"
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
    
    <!-- <div class="dp_top">
      <div class="logo">深圳市反诈技术支撑平台—{{ namenav }}</div>
      <div class="nav" v-show="$route.path==='/findUser'||$route.path==='/findRole'||$route.path==='/dept'">        
        <el-button
          size="mini"
          type="primary"
          class="navnum"
          v-for="(item, index) in daohangnum"
          :key="index"
          :class="['/' + item.name !== $route.path ? '' : 'yangshi']"
          @click="navclick(item.name, item.menuName)"
          >{{ item.menuName }}</el-button
        >
      </div>
    </div> -->
  </div>
</template>

<script>
// import { sendWebsocket, closeWebsocket } from "@/utils/websocket.js";
import dayjs from 'dayjs'
export default {
  // inject: ["reload"],
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
      curDate: dayjs().format("YYYY年MM月DD日"),
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
      newdomainSimpleVo: {
        oldpwd: '',
        xinpwd: '',
        xinpwd2: '',
      },
      yinc: false,
      dialog: false,
      fullscreen: false,
      message: 1,
      name: 'wu',
      path: '',
      msgType: '',
      use: '',
      old: '',
      daohangtitle: '',
      daohangnum: [
        { menuName: "角色管理", menuUrl: "/role/findRole",name: "findRole" },
        { menuName: "用户管理", menuUrl: "/user/findUser",name: "findUser" },
        
        { menuName: "日志管理", menuUrl: "/dept",name: "dept" }
      ],
      namenav: '',
    }
  },
  // created() {  // 7.4 测试暂时关闭接口
  //   // console.log(this.$route.path);
  //   // this.namenav=window.location.search
  //   this.namenav = this.$route.query.name

  //   this.daohang()
  // },
  // watch: {   // 7.4 测试有bug，暂时关掉
  //   $route: 'getPath',
  // },
  computed: {
    username() {
      let username = JSON.parse(window.sessionStorage.getItem('one'))
      // window.sessionStorage.setItem("userName", this.param.username);
      return username ? username : this.name
    },
    collapse() {
      return this.$store.state.collapse
    },
    pro() {
      return this.$store.state.webSocketMsg
    },
  },

  methods: {
    // getPath() {
    //   // this.namenav = this.$route.query.name
    //   this.daohang()  // 7.4 测试
    // },
    //头部动态导航
    navclick(val) {
      this.$router.push('/' + val)
      //  this.daohang()
    },

    sortNumber(a, b) {
      return a.sort - b.sort
    },

    // async daohang() {  // 7.4 测试暂时关闭接口
    //   this.daohangtitle = this.$route.path.slice(1, this.$route.path.length)
    //   const { data: res } = await this.$http.post('/menu/queryUserMenuList')
    //   if (res.code == 200) {
    //     for (var i = 0; i < res.data.length; i++) {
    //       if (res.data[i].children != null && res.data[i].children.length > 1) {
    //         for (var j = 0; j < res.data[i].children.length; j++) {
    //           if (this.daohangtitle == res.data[i].children[j].name) {
    //             this.namenav = res.data[i].menuName
    //             this.daohangnum = res.data[i].children
    //             this.daohangnum.sort(this.sortNumber)
    //         if(this.daohangnum[0].menuName=="4G原始流量"){
    //           this.daohangnum.reverse()
    //         }
    //           }
    //         }
    //       } else {
    //         if (this.daohangtitle == res.data[i].name) {
    //           this.namenav = res.data[i].menuName
    //           this.daohangnum = []
    //         }
    //       }
    //     }
    //   }
    // },
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
    err() {
      this.newdomainSimpleVo.oldpwd = ''
      let that = this
      this.$nextTick(() => {
        that.$refs['newdomainSimpleVo'].clearValidate()
      })
      this.dialog = false
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
    //-------------------------------------------------------------------------------------
    // 用户名下拉菜单选择事件
    handleCommand(command) {
      if (command == 'loginout') {
        // localStorage.removeItem("ms_username");
        this.tuichusession()
        window.sessionStorage.clear()

        this.$router.push('/home')
      } else if (command == 'upload') {
        this.dialog = true
        // this.newdomainSimpleVo.oldpwd = JSON.parse(
        //   window.sessionStorage.getItem('pwd')
        // )
      }
    },
    async tuichusession() {
      const { data: res } = await this.$http.post('/pagelogout')
    },
    ret1() {
      this.$router.push('/shouye')
    },
    handleClose1() {
      this.dialog = false
    },
    collapseChage() {
      this.$store.commit('hadndleCollapse', !this.collapse)
    },
  },
  mounted() {
    if (document.body.clientWidth < 1500) {
      this.collapseChage()
    }
  },
}
</script>
<style scoped lang='less'>
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
  height: 9.7%;
  position: relative;
}
.top1_title {
  height: 100px;
  width: 100%;
  background-image: url('../assets/img/newbg/header_bg.png');
  background-repeat: no-repeat;
  // background-size: cover;  // 不会让图片走形，尽可能完整
  background-size: 100% 100%; // 根据容器大小拉伸图片，会让图片走形
}
.title {
  text-align: center;
  font-family: 'heiti';
  height:75px;
  line-height: 75px;
  font-size: 42px;
  letter-spacing: 7px /* 12/16 */;
  color:#19c9d5;
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
.dialog-footer /deep/ .el-button-xitongerr {
  background: url(../assets/img/shouye/确定按钮.png) no-repeat;
  background-size: cover;
  border: none;
  color: #25c0fd;
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
.dialog-footer /deep/ .el-button-xitongup:focus,
.dialog-footer /deep/ .el-button-xitongup:hover {
  background: url(../assets/img/shouye/取消按钮.png) no-repeat;
  background-size: cover;
  border: none;
}
.item {
  position: absolute;
  top: -0.7rem;
  left: 1.4rem;
  margin-top: 0.5rem;
  margin-right: 2rem;
}
.header {
  position: relative;
  box-sizing: border-box;
  width: 100%;
  height: 70px;
  font-size: 22px;
  color: #fff;
  /* border-bottom: 1px solid #fff; */
}
.collapse-btn {
  float: left;
  padding: 0 21px;
  cursor: pointer;
  line-height: 70px;
}
img {
  float: left;
  /* width: 35px;
  height: 35px;
  margin: 15px; */
}
.header-right {
  float: right;
  padding-right: 20px;
}
.header-user-con {
  display: flex;
  height: 70px;
  align-items: center;
}
.btn-fullscreen {
  transform: rotate(45deg);
  margin-right: 5px;
  font-size: 24px;
}
.btn-bell,
.btn-fullscreen {
  position: relative;
  width: 30px;
  height: 30px;
  text-align: center;
  border-radius: 15px;
  cursor: pointer;
}
.btn-bell-badge {
  position: absolute;
  right: -8px;
  top: -2px;
  width: 8px;
  height: 8px;
  border-radius: 4px;
  background: #f56c6c;
  color: #fff;
}
.btn-bell .el-icon-bell {
  position: absolute;
  color: #fff;
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
.el-dropdown-link {
  position: relative;
  top:6px;
  color: #fff;
  cursor: pointer;
}
.el-dropdown-menu__item {
  text-align: center;
}
/* ++++++++++ */
.role {
  height: 30px /* 30/16 */;
}
.role img {
  height: 30px;
  /* margin-top:10px */
}
.nav {
  position: absolute;
  width: 500px;
  height: 50%;
  right: 10%;
  top: 20%;
}
.navnum {
  cursor: pointer;
  float: right;
  margin-left: 0 !important;
  margin-right: 1%;
  color: #ecf5ff;
  // background-color: #409eff80;
  // border-color: #1a5da0;
  background: url(../assets/img/shouye/未选中.png) no-repeat;
  background-size: 100% 100%;
  border: 0px;
}
.navnum:hover,
.navnum:focus {
  cursor: pointer;
  float: right;
  margin-left: 0 !important;
  margin-right: 1%;
  color: #ecf5ff;
  // background-color: #409eff80;
  // border-color: #1a5da0;
  background: url(../assets/img/shouye/选中1.png) no-repeat;
  background-size: 100% 100%;
  border: 0px;
}
//当前样式
.yangshi {
  background: url(../assets/img/shouye/选中1.png) no-repeat;
  background-size: 100% 100%;
}
.fanhui {
  position: absolute;
  left: 3.5%;
  top: 15%;
  color:#11d8e5;
  font-size: 20px;
}
.fanhui img {
  width: 75px;
  height: 40px;
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
/deep/ .search_select_form .el-form-item__label {
  color: #b9e6ff;
}
.jinzhi {
  pointer-events: none;
}
</style>
