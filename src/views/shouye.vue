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
        <div class="title">深圳市反诈技术支持平台</div>
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
      <!-- 头部_光条-->
      <div class="top1_line">
        <div class="top11_line"></div>
        <div class="top12_line">
          <div class="top12_line_son"></div>
        </div>
        <div class="top13_line"></div>
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
                :to="{ path: item.path, query: { name: item.name } }"
                class="six"
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
    <!-- 内容 -->
    <!-- <div class="center1">
      <div class="center1_left">
        <div class="left_Statistics">
          <div class="left_Statistics_title">
            <div class="wenzi">信息</div>
          </div>
          <div class="left_Statistics_center1">
            <div class="di"></div>
          </div>
        </div>
        <div class="left_type">
          <div class="left_Statistics_title">
            <div class="wenzi">总发现类型</div>
          </div>
          <div class="left_Statistics_center2">
            <div
              id="myChart"
              ref="echartsshouyehuanxing"
              style="width: 100%; height: 100%; position: relative"
            >
            </div>
            <div class="di"></div>
          </div>
        </div>
        <div class="left_trend">
          <div class="left_Statistics_title">
            <div class="youwenzi">
              <ul class="qiehuan">
                <li
                  v-on:click="qh1(true)"
                  class="Active jinzhi"
                  
                >
                  <span>预警</span>
                </li>
              </ul>
            </div>

            <div class="wenzi">总预警趋势</div>
          </div>
          <div class="left_Statistics_center3">
            <div
              id="myChart"
              ref="echartsshouye3zhexian"
              style="width: 100%; height: 100%"
            ></div>
            <div class="di"></div>
          </div>
        </div>
      </div>
      <div class="center1_map">
        <div class="nav">
          <div class="nav_li">
            <ul class="daohang3">
              <li
                v-for="(item, index) in daohang"
                :key="index"
              >
                <span>
                  <router-link
                    :to="{ path: item.path, query: { name: item.name } }"
                    class="six"
                    >{{ item.name }}</router-link
                  >
                </span>
              </li>
            </ul>

          </div>
        </div>
      </div>
      <div class="center1_right">
        <div class="left_huanbi">
          <div class="left_Statistics_title">
            <div class="wenzi">环比</div>
          </div>
          <div class="left_Statistics_center">
            <div class="di"></div>
          </div>
        </div>
        <div class="left_region">
          <div class="left_Statistics_title">
            <div class="wenzi">高危发现趋势感知</div>
          </div>
          <div class="left_Statistics_centerg" id="gaowei">
            <div id="myStackChart" ref="myStackChart" style="width: 100%; height: 100%;">
            </div>
            <div class="di"></div>
          </div>
        </div>
        <div class="left_information">
          <div class="left_Statistics_title">
            <div class="wenzi">信息详情</div>
          </div>
          <div class="left_Statistics_center">
            <div class="di"></div>
          </div>
        </div>
      </div>
    </div> -->

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
// import getDiscover from './getDiscover.vue'
import zhongwei from '../static/jsonmap/zhongwei.json'
import echarts from 'echarts'
import dayjs from 'dayjs'
import 'echarts-gl'
import { mapState, mapGetters, mapMutations } from 'vuex'
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

      //模块————高危发现趋势时间
      whiteSearchList1: {
        startCreateTime1:
          dayjs().subtract(1, 'month').format('YYYY/MM/DD') +
          ' ' +
          '00:' +
          '00:' +
          '00',
        endCreateTime1:
          dayjs().format('YYYY/MM/DD') + ' ' + '23:' + '59:' + '59',
      },
      whiteSearchListday: {
        startCreateTimeday: dayjs().format('YYYY/MM/DD'),
        endCreateTimeday: dayjs().format('YYYY/MM/DD'),
      },
      whiteSearchListdayzhe: {
        startCreateTimedayzhe:
          dayjs(this.timerstart(dayjs().format('YYYY/MM/DD'), 7)).format(
            'YYYY/MM/DD'
          ) +
          ' ' +
          '00:' +
          '00:' +
          '00',
        endCreateTimedayzhe:
          dayjs(this.timersend(dayjs().format('YYYY/MM/DD'), 1)).format(
            'YYYY/MM/DD'
          ) +
          ' ' +
          '23:' +
          '59:' +
          '59',
      },
      //信息
      numtongji: {
        chuzhinum: '',
        bocenum: '',
        yujinnum: '',
        qingbaonum: '',
        faxiannum: '',
      },
      //发现类型
      faxianleft: {
        faxiantitle: [],
        faxiantitle1: [],
      },
      faxianleftnum: {
        type: [],
        type1: [],
        percent: [],
      },
      title: [],
      //预警趋势
      yujingqs: {
        datanum: [],
        highTotalnum: [],
        inTotalnum: [],
        lowTotalnum: [],
      },
      chooseEquipment: {
        data: '',
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
      //居中今日
      daynum: {
        faxian: 0,
        CAfaxian: 0,
        yujing: 0,
        fanzhi: 0,
        fanzhi1: 0,
      },
      //环比zhongwei
      fanzhicount1: 0,
      fanzhipercent: '',
      yujingcount1: 0,
      yujingpercent: '',
      //高危发现趋势
      phonelist: [],
      //信息详情
      temp: true,
      temp1: true,
      listData: [],
      yulistData: [],
      //地图
      mock: [
        {
          name: '兴庆区',
          value: 0,
          coord: [106.278393, 38.46747],
        },
        {
          name: '西夏区',
          value: 0,
          coord: [106.132116, 38.492424],
        },
        {
          name: '金凤区',
          value: 0,
          coord: [106.228486, 38.477353],
        },
        {
          name: '永宁县',
          value: 0,
          coord: [106.253781, 38.28043],
        },
        {
          name: '贺兰县',
          value: 0,
          coord: [106.345904, 38.554563],
        },
        {
          name: '灵武市',
          value: 0,
          coord: [106.334701, 38.094058],
        },
        // 找不大
      ],
      mock1: [],
      mocklist: [],
      max: 0,
      min: 0,
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
  // mounted() {   
  //   this.$nextTick(()=>{
  //     this.drawStackChart(xdata,ydata)
  //   })
  // },
  // created() {
  //   this.dh()
  //   this.qx()
  //   this.houtai()
  //   this.tongji()
  //   this.faxianliexing()
  //   this.faxianqushi()
  //   this.yujing()
  //   // this.daohangqx()
  //   this.daytongji()
  //   this.huanbitongji()
  //   // this.victim() // 高危发现趋势
  //   this.fanzhilunbo()
  //   this.yujingnum()
  //   // this.daohangqx()
    
    
  // },

  computed: {
    // username() {
    //   let username = JSON.parse(window.sessionStorage.getItem('one'))
    //   return username ? username : this.name
    // },
    //滚动条
    defaultOption() {
      return {
        step: 0.5, // 数值越大速度滚动越快
        limitMoveNum: 2, // 开始无缝滚动的数据量 this.dataList.length
        hoverStop: true, // 是否开启鼠标悬停stop
        direction: 1, // 0向下 1向上 2向左 3向右
        openWatch: true, // 开启数据实时监控刷新dom
        singleHeight: 0, // 单步运动停止的高度(默认值0是无缝不停止的滚动) direction => 0/1
        singleWidth: 0, // 单步运动停止的宽度(默认值0是无缝不停止的滚动) direction => 2/3
        waitTime: 1000, // 单步运动停止的时间(默认值1000ms)
      }
    },
    ...mapState(['map']),
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
    //右上角用户
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
    // err() {
    //   this.newdomainSimpleVo.oldpwd = ''
    //   let that = this
    //   this.$nextTick(() => {
    //     that.$refs['newdomainSimpleVo'].clearValidate()
    //   })
    //   this.dialog = false
    // },
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

    // 预警数据接口
    // async yujingnum() {  // 7.4 测试暂时关闭接口
    //   const { data: res } = await this.$http.get(
    //     '/warning_4g/homePageSmallStatistic'
    //   )
    //   if (res.code == 200) {
    //     this.numtongji.yujinnum = res.data.totalNum4g
    //     this.numtongji.bocenum = res.data.totalNumCa
    //     this.daynum.fanzhi1 = res.data.todayWarningNum4g
    //     this.daynum.yujing = res.data.todayWarningNumCa
    //     this.yujingcount1 = res.data.thisMonthTotal

    //     if (res.data.chainLastMonth == '上月无数据') {
    //       this.yujingpercent = '-'
    //     } else {
    //       this.yujingpercent = (res.data.chainLastMonth * 100).toFixed(2)
    //     }
    //   }
    // },

    // 模块————
    // async tongji() {   // 7.4 测试暂时关闭接口
    //   const { data: res } = await this.$http.get(
    //     '/discoverStatistic/getTotalByTimeRange',
    //     {
    //       params: {
    //         startTime: null,
    //         endTime: null,
    //       },
    //     }
    //   )
    //   if (res.code == 200) {
    //     if (Object.getOwnPropertyNames(res.data).length != 0) {
    //       this.numtongji.chuzhinum = res.data.CADISCVOER
    //       this.numtongji.faxiannum = res.data.OUTNETDISCOVER
    //     } else {
    //       this.numtongji.chuzhinum = 0
    //       this.numtongji.faxiannum = 0
    //     }
    //   }
    // },
    // 模块————高危发现趋势感知
    // async faxianqushi(){  // 7.4 测试暂时关闭接口
    //   const { data:res } = await this.$http.get('/discoverStatistic/weekStatistics')
    //   if(res.code == 200){
    //     const xData = [],yData = [] 
    //     if(res.data.length > 0){         
    //       res.data.forEach((item) => {
    //         xData.push(item.date)
    //         yData.push(item.cnt)            
    //       })
    //       this.drawStackChart(xData,yData)
    //     } else {
    //       var html = `<span style="font-size:16px;color:#fff;margin-left:10px;margin-top:10px"></span><span style="margin-top:10px;margin-left:15px;color:#fff;font-size:12px"></span><br/><span class="echarts-nodate" style="color:#fff;margin-left:10px;">暂无数据</span>`
    //       document.getElementById('myChart').innerHTML = html
    //       document
    //         .getElementById('myChart')
    //         .removeAttribute('_echarts_instance_')

    //       return
    //     }
    //   }      
    // },
    // 模块————总发现类型
    // async faxianliexing() {  // 7.4 测试暂时关闭接口
    //   const { data: res } = await this.$http.get(
    //     '/discoverStatistic/getLargeTypeStatistic',
    //     {
    //       params: {
    //         startTime: this.whiteSearchList1.startCreateTime1,
    //         endTime: this.whiteSearchList1.endCreateTime1,
    //       },
    //     }
    //   )
    //   if (res.code == 200) {
    //     if (res.data.length > 0) {
    //       res.data.forEach((item) => {
    //         if (item.type == 'SZP') {
    //           item.type = '杀猪盘'
    //         } else if (item.type == 'KF') {
    //           item.type = '冒充客服类'
    //         } else if (item.type == 'DK') {
    //           item.type = '贷款'
    //         } else if (item.type == 'SD') {
    //           item.type = '刷单返利类'
    //         } else if (item.type == 'GJF') {
    //           item.type = '冒充公检法类'
    //         } else if (item.type == 'HC') {
    //           item.type = '灰产'
    //         }else if (item.type == 'LC') {
    //         item.type = '理财'
    //       }else if (item.type == 'DS') {
    //         item.type = '电商类诈骗'
    //       } else if (item.type == 'JJGW') {
    //           item.type = '冒充军警购物诈骗'
    //         } else if (item.type == 'JY') {
    //           item.type = '网络婚恋、交友类'
    //         } else if (item.type == 'ZX') {
    //           item.type = '虚假征信类'
    //         } else if (item.type == 'MC') {
    //           item.type = '冒充领导、熟人类'
    //         } else if (item.type == 'YX') {
    //           item.type = '网络游戏产品虚假交易类'
    //         } else if (item.type == 'OTHER') {
    //           item.type = '其他类型诈骗'
    //         } else if (item.type == 'APP') {
    //           item.type = '分发平台'
    //         } else if (item.type == 'XZYM') {
    //           item.type = '下载页面'
    //         }
    //       })
    //       res.data = res.data.map((item) => {
    //         return {
    //           name: item.type,
    //           value: item.count,
    //           percent: item.percent,
    //         }
    //       })
    //       res.data.forEach((item) => {
    //         this.title.push(item.name)
    //       })
    //       this.res = res.data

    //       if (this.$refs.echartsshouyehuanxing) {
    //         this.drawLine()
    //       }
    //     } else {
    //       var html = `<span style="font-size:16px;color:#fff;margin-left:10px;margin-top:10px"></span><span style="margin-top:10px;margin-left:15px;color:#fff;font-size:12px"></span><br/><span class="echarts-nodate" style="color:#fff;margin-left:10px;">暂无数据</span>`
    //       document.getElementById('myChart').innerHTML = html
    //       document
    //         .getElementById('myChart')
    //         .removeAttribute('_echarts_instance_')

    //       return
    //     }
    //   }
    // },
    drawLine() {
      var that = this
      // eslint-disable-next-line camelcase
      var bar_qx = this.$refs.echartsshouyehuanxing
      // eslint-disable-next-line camelcase

      let myChart = this.$echarts.init(bar_qx)
      window.addEventListener('resize', function () {
        myChart.resize()
      })

      myChart.on('mouseover', function (event) {
        var name = event.data.name
        var value = event.percent

        option1.title[1].text = value.toFixed(0) + '%'
        option1.title[1].textStyle = {
          fontSize: 14,
          align: 'center',
          fontWeight: 'bold',
          color: event.color,
        }
        myChart.setOption(option1)
      })

      // },

      // setOption() {
      var colorList = [
        '#7ed4ac',
        '#fe9c3a',
        '#ebd571',
        '#eb7c6f',
        '#0bb4d7',
        '#8fcde5',
        '#00c973',
      ]
      let option1 = {
        tooltip: {
          trigger: 'item',
          // textStyle: {
          //   fontSize: 9, // 调整字号大小
          //   lineHeight: 24, // 调整行高
          // },
          position: ['50%', '50%'], // 将 tooltip 定位到图表中心
          // padding: 10, // 调整 tooltip 内容与边框的间距
          // extraCssText: 'max-width: 80vw; max-height: 80vh; overflow: auto;', // 调整 tooltip 的最大宽度和高度，并添加滚动条
          formatter: function (val) {
            // console.log(val)
            return (
              '<span>类型:</span><br> <i style="display: inline-block;width: 10px;height: 10px;background: ' +
              val.color +
              ';margin-right: 5px;border-radius: 50%;}"></i><span width:70px; display:inline-block;>' +
              val.name +
              ':' +
              '</span>' +
              '<span>' +
              val.percent.toFixed(0) +
              '%' +
              '</span>'
            )
          },
        },

        color: colorList,
        backgroundColor: 'transparent',

        legend: {
          show: false,
          type: 'scroll',
          orient: 'horizontal',
          icon: 'square',
          top: '80%',
          left: 'center',
          align: 'auto',
          textStyle: {
            color: '#fff',
          },
        },

        title: [
          {
            textStyle: {},
            text: '',
          },
          {
            top: 'center',
            right: 'center',
          },
        ],
        series: [
          {
            hoverAnimation: true,
            // name: 'Access From',
            type: 'pie',
            // radius: '60%',
            radius: ['30%', '50%'],
            center: ['50%', '50%'],
            data: this.res,
            itemStyle: {
              normal: {
                borderColor: colorList,
                borderWidth: 0,
              },
            },
            labelLine: {
              normal: {
                lineStyle: {
                  color: '#e6e6e6',
                },
              },
            },
            label: {
              normal: {
                formatter: '{name|{b}}',

                rich: {
                  icon: {
                    fontSize: 12,
                  },
                  name: {
                    fontSize: 10,
                    padding: [0, 0, 0, 0],
                  },
                },
              },
            },
          },
        ],
      }
      myChart.setOption(option1)
    },
    //   return option1
    // },
    // 模块————预警趋势  ______________________________________
    // async yujing() {  // 7.4 测试接口暂时关闭
    //   this.yujingqs.datanum = []
    //   this.yujingqs.gaoweinum = []
    //   this.yujingqs.inTotalnum = []
    //   this.yujingqs.lowTotalnum = []
    //   let list = {
    //     endTime: this.whiteSearchListdayzhe.endCreateTimedayzhe,
    //     startTime: this.whiteSearchListdayzhe.startCreateTimedayzhe,
    //   }
    //   const { data: res } = await this.$http.post(
    //     '/warning_4g/Statistic4GWarningNumsByEarlyGrade',
    //     list
    //   )
    //   if (res.code == 200) {
    //     for (var i = 0; i < res.data.length; i++) {
    //       this.yujingqs.datanum.push(
    //         dayjs(res.data[i].queryDate).format('MM/DD')
    //       )
    //       this.yujingqs.gaoweinum.push(res.data[i].gw)
    //       this.yujingqs.inTotalnum.push(res.data[i].zw)
    //       this.yujingqs.lowTotalnum.push(res.data[i].dw)
    //     }
    //     if (this.$refs.echartsshouye3zhexian) {

    //       this.drawLinezhexian()
    //     }
    //   }
    // },
    async yujing1() {
      this.yujingqs.datanum = []
      this.yujingqs.gaoweinum = []
      this.yujingqs.inTotalnum = []
      this.yujingqs.lowTotalnum = []
      let list = {
        endTime: this.whiteSearchListdayzhe.endCreateTimedayzhe,
        startTime: this.whiteSearchListdayzhe.startCreateTimedayzhe,
      }
      const { data: res } = await this.$http.post(
        '/warning_ca/StatisticCAWarningNumsByEarlyGrade',
        list
      )
      if (res.code == 200) {
        for (var i = 0; i < res.data.length; i++) {
          this.yujingqs.datanum.push(
            dayjs(res.data[i].queryDate).format('MM/DD')
          )
          this.yujingqs.gaoweinum.push(res.data[i].gw)
          this.yujingqs.inTotalnum.push(res.data[i].zw)
          this.yujingqs.lowTotalnum.push(res.data[i].dw)
        }
        if (this.$refs.echartsshouye3zhexian) {
          // this.loading1 = false

          this.drawLinezhexian()
        }
      }
    },
    // 新增堆叠图
    drawStackChart(xdata,ydata){
      xdata = xdata.map((item)=>{
        const temp = item.split('-')
        item = temp[1]+'/'+temp[2]
        return item
      })
      var stack_qx = this.$refs.myStackChart;
      let myStackChart1 = this.$echarts.init(stack_qx)
      window.addEventListener('resize', function () {
        myStackChart1.resize()
        // console.log(1)
      })
      myStackChart1.clear()

      let option = {
        tooltip: {
          trigger: 'axis',

          axisPointer: {
            lineStyle: {
              color: '#66B3FF',
            },
          },
        },

        grid: {
            top:'15%',
            left: '8%',
            right: '8%',
            bottom: '8%',
            containLabel: true
        },
        xAxis: [
            {
                type: 'category',
                boundaryGap: false,
                data: xdata,
                axisLine:{
                  lineStyle:{
                    color:'#666',
                    width:1
                  }
                },
                axisLabel:{
                  textStyle:{
                    color:'#f7ffff'
                  }
                },
                // splitLine:{
                //   show:false
                // }
                // splitLine: {
                  // lineStyle:{
                  //     color: ['#80a2c0'],
                  //     width: 1,
                  //     type: 'solid'
                  //     }
                //   }                
            }
        ],
        yAxis: [
            {
                type: 'value',
                axisLine:{
                  show:false,
                  // lineStyle:{
                  //   color:'#80a2c0'
                  // }
                },
                axisTick:{show:false},
                splitLine: { show: false },
                splitLine: {
                  lineStyle: {
                    color: ['#1e324a'],
                  },
                },
                axisLabel:{
                  textStyle:{
                    color:'#828d9a'
                  }
                }
            }
        ],
        series: [
              {
              data: ydata,
              type: 'line'
          }
        ]
      };
      myStackChart1.setOption(option)
      
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

    //导航
    // async daohangqx() {
    //   const that = this
    //   const { data: res } = await this.$http.get('/menu/queryUserNavList')
    //   if (res.code == 200) {
    //     // console.log(res.data)
    //     res.data = res.data.map((item) => {
    //       return {
    //         name: item.menuName,
    //         path: item.menuUrl,
    //       }
    //     })
    // //     //排序
    // //     // res.data.splice(0, 0, res.data[res.data.length - 1])
    // //     // res.data.splice(res.data.length - 1, 1)
    //     this.daohang = res.data
    //      window.localStorage.setItem('nav', JSON.stringify(this.daohang))
    //   }
    // },
    //   // this.items = res.data;
    // },
    // 点击上下方法
    remove(array, index) {
      if (index <= array.length - 1) {
        for (var i = index; i < array.length; i++) {
          array[i] = array[i + 1]
        }
      }
      array.length = array.length - 1

      return array
    },
    // 模块————导航上
    // up() {
    //   if (this.daohang.length == 1) {
    //     this.$message('当前您的权限只有一项')
    //   } else {
    //     var arr = this.daohang.slice(0, 1)
    //     this.daohang = this.remove(this.daohang, 0)
    //     this.daohang = [...this.daohang, ...arr]
    //   }

    //   // console.log(this.daohang);
    // },
    // 模块————导航下
    low() {
      if (this.daohang.length == 1) {
        this.$message('当前您的权限只有一项')
      } else {
        var arr = this.daohang.slice(
          this.daohang.length - 1,
          this.daohang.length
        )
        this.daohang = this.remove(this.daohang, this.daohang.length)
        // console.log( this.daohang);
        this.daohang = [...arr, ...this.daohang]
      }
    },
    // 模块————今日实时
    // async daytongji() {   // 7.4 测试暂时关闭接口
    //   const { data: res } = await this.$http.get(
    //     '/discoverStatistic/getTotalByTimeRange',
    //     {
    //       params: {
    //         startTime: this.whiteSearchListday.startCreateTimeday,
    //         endTime: this.whiteSearchListday.endCreateTimeday,
    //       },
    //     }
    //   )
    //   if (res.code == 200) {
    //     if (Object.getOwnPropertyNames(res.data).length != 0) {
    //       this.daynum.CAfaxian = res.data.CADISCVOER
    //     } else {
    //       this.daynum.CAfaxian = 0
    //     }
    //   }
    // },
    // 模块————环比
    // async huanbitongji() {   // 7.4 测试暂时关闭接口
    //   const { data: res } = await this.$http.get(
    //     '/discoverStatistic/getMonthOnMonth'
    //   )
    //   if (res.code == 200) {
    //     this.fanzhicount1 = res.data[0].count1
    //     this.fanzhipercent = res.data[0].percent
    //     if (this.fanzhipercent == '上期没有数据') {
    //       this.fanzhipercent = '-'
    //     }
    //   }
    // },
    // 模块————高危发现趋势
    // async victim() {  // 7.4 测试暂时关闭接口
    //   const { data: res } = await this.$http.get(
    //     '/warning_4g/statisticAllVictimAddress'
    //   )
    //   if (res.code == 200) {
    //     if (res.data.length > 0) {
    //       this.phonelist = res.data
    //         .map((item) => {
    //           return {
    //             address: item.statisticKey,
    //             visits: item.statisticValue,
    //           }
    //         })
    //         .slice(0, 5)
    //     } else {
    //       var html = `<span style="font-size:16px;color:#fff;margin-left:10px;margin-top:10px"></span><span style="margin-top:10px;margin-left:15px;color:#fff;font-size:12px"></span><br/><span class="echarts-nodate" style="color:#fff;margin-left:10px;">暂无数据</span>`
    //       document.getElementById('gaowei').innerHTML = html
    //       document
    //         .getElementById('gaowei')
    //         .removeAttribute('_echarts_instance_')

    //       return
    //     }
    //   }
    // },
    //动态渲染class
    generateClassName(index) {
      return 'right_num' + (index + 1)
    },
    generateClassNamezhu(index) {
      return 'btm_color' + (index + 1)
    },
    generateClassNamecir(index) {
      return 'cir' + (index + 1)
    },
    // 模块————预警反制切换
    qh(t) {
      this.temp = t
      if (this.temp == false) {
        this.yujinglunbo()
      } else {
        this.fanzhilunbo()
      }
    },

    // 模块————预警4g长安切换
    qh1(t) {
      // this.loading1 = true
      this.temp1 = t

      if (this.temp1 == false) {
        this.yujingqs.datanum = []
        this.yujingqs.gaoweinum = []
        this.yujingqs.inTotalnum = []
        this.yujingqs.lowTotalnum = []

        this.yujing1()
      } else {
        this.yujingqs.datanum = []
        this.yujingqs.gaoweinum = []
        this.yujingqs.inTotalnum = []
        this.yujingqs.lowTotalnum = []

        this.yujing()
      }
    },
    // 模块————反制信息
    // async fanzhilunbo() {  // 7.4 测试暂时关闭接口
    //   const { data: res } = await this.$http.post('/discover/getDiscoverFirst')
    //   if (res.code == 200) {
    //     res.data.forEach((item) => {
    //       if (item.type == 'SZP') {
    //         item.type = '杀猪盘'
    //       } else if (item.type == 'KF') {
    //         item.type = '冒充客服类'
    //       } else if (item.type == 'DK') {
    //         item.type = '贷款代办信用卡类'
    //       } else if (item.type == 'SD') {
    //         item.type = '刷单返利类'
    //       } else if (item.type == 'GJF') {
    //         item.type = '冒充公检法类'
    //       } else if (item.type == 'HC') {
    //         item.type = '灰产'
    //       } else if (item.type == 'LC') {
    //         item.type = '理财'
    //       }else if (item.type == 'DS') {
    //         item.type = '电商类诈骗'
    //       }else if (item.type == 'JJGW') {
    //         item.type = '冒充军警购物诈骗'
    //       } else if (item.type == 'JY') {
    //         item.type = '网络婚恋、交友类'
    //       } else if (item.type == 'ZX') {
    //         item.type = '虚假征信类'
    //       } else if (item.type == 'MC') {
    //         item.type = '冒充领导、熟人类'
    //       } else if (item.type == 'YX') {
    //         item.type = '网络游戏产品虚假交易类'
    //       } else if (item.type == 'OTHER') {
    //         item.type = '其他类型诈骗'
    //       } else if (item.type == 'APP') {
    //         item.type = '分发平台'
    //       } else if (item.type == 'XZYM') {
    //         item.type = '下载页面'
    //       }
    //     })

    //     this.listData = res.data
    //   }
    // },
    // 模块————预警轮播
    async yujinglunbo() {
      const { data: res } = await this.$http.get('/warning_4g/selectCATop100')
      if (res.code == 200) {
        console.log(res.data);
        this.yulistData = res.data
      }
    },
    //模块————地图
    houtai() {
      // //有值直接执行
      // if (this.$store.state.map != '') {
      //   this.maplist = this.$store.state.map
      //   this.mapquan1()
      // } else {
      //   //无值请求
      //   this.$http
      //     .get(
      //       'http://10.8.0.120:8088/geoserver/test/ows?service=WFS&version=1.0.0&request=GetFeature&typeName=test%3A%E6%88%90%E9%83%BD%E5%8C%BA%E5%8E%BF&maxFeatures=50&outputFormat=application%2Fjson'
      //     )
      //     .then((res) => {

      //       this.maplist = res.data
      //       this.$store.commit('mapread', this.maplist)
      //       this.mapquan1()
      //     })
      // }
      // this.maplist = yinchuan
      this.maplist = zhongwei
      this.mapquan1()
    },
    //删除key
    /*
     *@param {Array} target 目标数组
     *@param {Array || String} keys 删除的key
     */
    deleteJsonKey(target, keys) {
      if (target.length == 0) {
        return target
      }
      let result = []
      if (Array.isArray(keys)) {
        let mixed = Object.keys(...target).filter((x) => !new Set(keys).has(x))
        result = JSON.parse(JSON.stringify(target, mixed))
      } else {
        result = JSON.parse(
          JSON.stringify(target, (k, v) => {
            if (k !== keys) {
              return v
            }
          })
        )
      }
      return result
    },
    //模块————地图
    // async mapquan1() {   // 7.4 测试暂时关闭接口
    //   const { data: res } = await this.$http.get(
    //     '/warning_4g/statisticAllVictimAddress'
    //   )
    //   if (res.code == 200) {
    //     if (res.data.length > 0) {
    //       for (var i = 0; i < res.data.length; i++) {
    //         for (var j = 0; j < this.mock.length; j++) {
    //           if (this.mock[j].name == res.data[i].statisticKey) {
    //             this.mock[j].value = res.data[i].statisticValue
    //           }
    //         }
    //       }
    //       for (var j = 0; j < this.mock.length; j++) {
    //         this.mock[j].name = this.mock[j].name
    //       }

    //       this.mock.map((item) => {
    //         return {
    //           name: item.name,
    //           value: item.value,
    //           coord: item.coord,
    //         }
    //       })

    //       this.mock1 = this.mock
    //       this.mocklist = this.mock

    //       this.mocklist = this.deleteJsonKey(this.mocklist, ['coord'])
    //       var arr = []
    //       for (var i = 0; i < this.mock1.length; i++) {
    //         arr.push(this.mock1[i].value)
    //       }
    //       this.max = Math.max(...arr)

    //       this.min = Math.min(...arr)
    //       if (this.$refs.chartmap) {
    //         this.mapcity()
    //       }
    //     } else {
    //       this.mocklist = this.mock
    //       if (this.$refs.chartmap) {
    //         this.mapcity()
    //       }
    //     }
    //   }
    // },
    //中间地图
    mapcity() {
      var el = this.$refs.chartmap
      var myChart2 = this.$echarts.init(el)

      this.$echarts.registerMap('YC', this.maplist)
      window.addEventListener('resize', function () {
        myChart2.resize()
        // console.log(1)
      })

      myChart2.setOption(this.new())
      var that = this
      // myChart2.on('click', function (params) {
      //   console.log(params)

      //   that.$router.push({
      //     path: '/taishi',
      //     query: { citynames: params.name, name: '情报感知' },
      //   })
      //   //此处写点击事件内容
      // })
    },
    new() {
      let option = {
        geo: [
          {
            map: 'YC',
            show: true,
            geoIndex: 1,
            aspectScale: 0.95, //长宽比
            zoom: 1.2,
            roam: false,
            silent: true,

            label: {
              normal: {
                show: false,
              },
              emphasis: {
                show: false,
              },
            },
            itemStyle: {
              normal: {
                borderColor: 'rgb(47, 79, 123)', // 图形的描边颜色 #0AAEB0
                borderWidth: 1, // 描边线宽。
                borderType: 'solid', // 柱条的描边类型。
                areaColor: 'rgb(35, 63, 83)', // 图形的颜色 #233F53

                label: {
                  show: false, // 显示区域名称
                  textStyle: {
                    color: '#fff',
                    fontSize: 12,
                  },
                },
              },
              color: 'rgba(3,42,76,.9)', //地图颜色
              borderWidth: 1, //分界线wdith
              borderColor: '#ccc', //分界线颜色
              opacity: 1,
            },

            layoutCenter: ['44%', '51%'], //右下
            layoutSize: '80%',
          },
          {
            map: 'YC',
            show: true,
            geoIndex: 1,

            aspectScale: 0.95, //长宽比
            zoom: 1.2,
            roam: false,
            silent: true,

            label: {
              normal: {
                show: false,
              },
              emphasis: {
                show: false,
              },
            },
            itemStyle: {
              normal: {
                borderColor: 'rgb(47, 79, 123)', // 图形的描边颜色 #0AAEB0
                borderWidth: 3, // 描边线宽。
                borderType: 'solid', // 柱条的描边类型。
                areaColor: 'rgb(35, 63, 83)', // 图形的颜色 #233F53

                label: {
                  show: false, // 显示区域名称
                  textStyle: {
                    color: '#fff',
                    fontSize: 12,
                  },
                },
              },
              color: 'rgba(3,42,76,.9)', //地图颜色
              borderWidth: 3, //分界线wdith
              borderColor: 'red', //分界线颜色
              opacity: 2,
            },

            layoutCenter: ['45%', '50%'], //右下
            layoutSize: '80%',
            emphasis: {
              show: false,
            },
          },
        ],
        // tooltip: {
        //   // trigger: "item",
        //   trigger: 'item',
        //   backgroundColor: 'rgba(3,54,92,.5)',
        //   borderWidth: 0,
        //   formatter: function (val) {
        //     // if (val.data.value != 0) {
        //     return val.data.name + '<br>' + '预警量：' + val.data.value
        //     // }
        //   },
        //   // formatter: {b} +':' +{c},
        //   // formatter:function(val){
        //   //   console.log(val);
        //   // },
        //   axisPointer: {
        //     animation: false,
        //   },
        //   show: true,
        // },
        visualMap: {
          type: 'piecewise', // 类型为分段型
          splitNumber: 4,
          show: false, //图注

          pieces: [
            // 自定义每一段的范围，以及每一段的文字
            {
              min: (this.max / 5) * 4,
              max: this.max,
              color:
                (this.max / 5) * 4 != 0
                  ? 'rgb(209, 77, 94)'
                  : 'rgb(64, 128, 207)',
            }, // 不指定 max，表示 max 为无限大（Infinity）。

            {
              min: (this.max / 5) * 3,
              max: (this.max / 5) * 4,
              color:
                (this.max / 5) * 3 != 0
                  ? 'rgb(255, 145, 25)'
                  : 'rgb(64, 128, 207)',
            },
            {
              min: (this.max / 5) * 2,
              max: (this.max / 5) * 3,
              color:
                (this.max / 5) * 2 != 0
                  ? 'rgb(194, 197, 15)'
                  : 'rgb(64, 128, 207)',
            },
            {
              min: this.max / 5,
              max: (this.max / 5) * 2,
              color:
                this.max / 5 != 0 ? 'rgb(5, 224, 253)' : 'rgb(64, 128, 207)',
            },

            { min: 0, max: this.max / 5, color: 'rgb(64, 128, 207)' },

            // 不指定 min，表示 min 为无限大（-Infinity）。
            // padding: [0, 0, 40, 20],
            // textStyle: {
            //   color: 'white',
            // },
            // show: false,

            // calculable: true,
            // align: 'auto',
            // realtime: false,
            //  min: this.min,
            //         max: this.max,
            //         text: ['最高值', '最低值'],

            //         inRange: {
            //              color: ['#2d88eb', '#007aff', '#ff9119', '#d14759'],
            //         },
          ],
        },

        series: [
          {
            map: 'YC',
            type: 'map',
            silent:true,   // 禁用地图hover事件
            aspectScale: 0.95, //长宽比
            zoom: 1.2,
            roam: false,
            componentType: 'geo',
            layoutSize: '80%', //大小
            layoutCenter: ['46%', '49%'],
            label: {
              show: true,
              textStyle: {
                color: '#fff',
                fontSize: 12,
              },
            },

            itemStyle: {
              normal: {
                borderColor: 'rgb(125, 187, 253)', // 图形的描边颜色 #0AAEB0
                borderWidth: 3, // 描边线宽。
                borderType: 'solid', // 柱条的描边类型。
                areaColor: 'rgb(64, 128, 207)', // 图形的颜色 #233F53

                label: {
                  show: true, // 显示区域名称
                  textStyle: {
                    color: '#fff',
                    fontSize: 12,
                  },
                },
              },
              // 鼠标移入时
              emphasis: {
                borderColor: 'rgb(118, 226, 255)', // 图形的描边颜色 #2378f7
                borderWidth: '3',
                areaColor: 'rgb(0,102,156)', // 阴影色 #233F53
                label: {
                  show: false,
                  textStyle: {
                    color: '#fff',
                    fontSize: 14,
                  },
                },
              },

              effect: {
                show: false,
                shadowBlur: 10,
                loop: true,
              },
            },

            data: this.mocklist,
          },
        ],
      }

      this.loading = false

      return option
    },
    // 状态判断
    state1(val) {
      let states = ''
      switch (val) {
        case 0:
          states = '处置中'
          break
        case 1:
          states = '已处置'
          break
        case 2:
          states = '已失效'
          break
      }
      return states
    },
    //系统权限
    // async qx() {  // 7.4  测试暂时关闭接口
    //   const that = this
    //   const { data: res } = await this.$http.post('/menu/queryUserMenuList')
    //   if (res.code == 200) {
    //     let chuarr = res.data.filter((item) => item.menuName != '系统管理')
    //     let chuarr1 = res.data.filter((item) => item.menuName == '系统管理')
    //     for (var i = 0; i < chuarr1.length; i++) {
    //       chuarr.push(chuarr1[0])
    //     }
    //     this.items = chuarr
    //     chuarr.forEach((item) => {
    //       that.menuFir(item)
    //     })
    //     this.pid = this.getSetArr(this.pid)
    //     if (!window.sessionStorage.getItem('btn')) {
    //       window.sessionStorage.setItem('btn', this.Newname1)
    //     }
    //     chuarr.forEach((item) => {
    //       that.menuSec(item)
    //     })
    //   }
    // },
    getSetArr(arr) {
      return [...new Set(arr)]
    },
    //
    menuFir(data) {
      if (data.menuType != 1) {
        // data["isShow"] = true;
        if (data.children) {
          data.children.forEach((item) => {
            this.menuFir(item)
          })
        }
      } else {
        // data["isShow"] = false;
        this.pid.push(data.pid)
        this.Newname.push(data.name)
        this.Newname1 = this.Newname
      }
    },

    menuSec(data) {
      var flag = false
      for (var item in this.pid) {
        // console.log(item);
        // console.log(data.id,this.pid[item]);
        if (data.id != this.pid[item]) {
          flag++
          continue
        } else {
          data.menuType = 1
          break
        }
      }
      if (data.children) {
        data.children.forEach((item) => {
          this.menuSec(item)
        })
      } else {
        data.menuType = 1
      }
    },
    async daohangqx() {
      const that = this
      const { data: res } = await this.$http.get('/menu/queryUserNavList')
      if (res.code == 200) {
        // console.log(res.data)
        res.data = res.data.map((item) => {
          return {
            name: item.menuName,
            path: item.menuUrl,
          }
        })
        res.data.forEach((item) => {
          if (item.name == '验证' || item.name == '处置') {
            item.path = ''
          }
        })
        //排序
        this.daohang = res.data
        window.localStorage.setItem('nav', JSON.stringify(this.daohang))
      }

      // this.items = res.data;
    },
    ret1() {
      this.$router.push('/home')
      window.localStorage.clear()
      window.sessionStorage.clear()
    },
    // 左一时间  开始时间
    timerstart(val, num) {
      let timelist = dayjs(val).subtract(num, 'day')
      return dayjs(timelist.$d).format('YYYY-MM-DD')
      // console.log(timelist);
    },
    timersend(val, num) {
      let timelist = dayjs(val).subtract(num, 'day')
      return dayjs(timelist.$d).format('YYYY-MM-DD')
      // console.log(timelist);
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
  background-image: url('../assets/newimg/newhome/背景.png');
  background-repeat: no-repeat;
  background-size: 100% 100%;
}
.top1 {
  width: 100%;
  height: 8.7%;
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
  height:65px;
  line-height: 65px;
  font-size: 34px;
  letter-spacing: 5px /* 12/16 */;
  // 实现文字渐变 background-image、-webkit-background-clip、-webkit-text-fill-color
  background-image: linear-gradient(0deg, #f8f8fb 0%, #8ec4fd 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  text-shadow: 0 0 20px rgba(0, 121, 255, 0.5);
}
.top1_title {
  height: 65px;
  width: 100%;
  background: url('../assets/newimg/newhome/顶部.png') no-repeat 100% 100%;
  // background-position: 0;
  background-size: cover;
}
.top1_line {
  height: 10px;
  width: 100%;
  // background: url('../assets/newimg/newhome/光.png') no-repeat;
  // background-size: 100% 100%;
}
.top11_line,
.top12_line,
.top13_line {
  float: left;
}
.top11_line,
.top13_line {
  width: 10%;
  height: 10px;
}
.top12_line {
  width: 80%;
  height: 10px;
  position: relative;
  overflow: hidden;
}
.top12_line_son {
  position: absolute;
  left: -100%;
  top: 0;
  width: 80%;
  height: 10px;
  background: url('../assets/newimg/newhome/光.png') no-repeat;
  background-size: 100% 100%;
  animation: myfirst 5s linear normal;
  animation-iteration-count: infinite;
}
@keyframes myfirst {
  0% {
    left: -80%;
    top: 0px;
  }
  10% {
    left: -60%;
    top: 0px;
  }
  20% {
    left: -40%;
    top: 0px;
  }
  30% {
    left: -20%;
    top: 0px;
  }
  40% {
    left: 0%;
    top: 0px;
  }
  50% {
    left: 20%;
    top: 0px;
  }
  60% {
    left: 40%;
    top: 0px;
  }
  70% {
    left: 60%;
    top: 0px;
  }
  80% {
    left: 80%;
    top: 0px;
  }
  90% {
    left: 90%;
    top: 0px;
  }
  100% {
    left: 100%;
    top: 0px;
  }
}
@keyframes mysecond {
  0% {
    -webkit-transform: rotate(0deg);
  }
  25% {
    -webkit-transform: rotate(90deg);
  }
  50% {
    -webkit-transform: rotate(180deg);
  }
  75% {
    -webkit-transform: rotate(270deg);
  }
  100% {
    -webkit-transform: rotate(360deg);
  }
}
@keyframes mythree {
  0% {
    -webkit-transform: rotate(360deg);
  }
  25% {
    -webkit-transform: rotate(270deg);
  }
  50% {
    -webkit-transform: rotate(180deg);
  }
  75% {
    -webkit-transform: rotate(90deg);
  }
  100% {
    -webkit-transform: rotate(0deg);
  }
}
@keyframes myfour {
  0% {
    opacity: 0.6;
  }
  25% {
    opacity: 0.8;
  }
  50% {
    opacity: 1;
  }
  75% {
    opacity: 0.8;
  }
  100% {
    opacity: 0.6;
  }
}
// @keyframes myfive {
//     0%{bottom: 24%;}
//       25%{bottom: 25%;}
//       50%{bottom: 26%;}
//      75%{bottom: 25%;}
//         100%{bottom: 24%;}
// }
@keyframes myfive {
  0% {
    left: -2%;
  }
  25% {
    left: -1%;
  }
  50% {
    left: 0%;
  }
  75% {
    left: -1%;
  }
  100% {
    left: -2%;
  }
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

.left_Statistics {
  width: 100%;
  // height: 0;
  flex: 1;
  // margin-bottom: 26px;
}
.left_Statistics_title {
  width: 100%;
  // height: 15%;
  height: 30px;
  background: url('../assets/newimg/newhome/标题装饰.png') no-repeat;
  background-size: 100% 100%;
  padding: 0 0 0 50px;
  box-sizing: border-box;
  position: relative;
}
.wenzi {
  position: absolute;
  width: 40%;
  height: 30px;
  top: 50%;
  transform: translateY(-50%);
  // letter-spacing: 2px;
  font-weight: 400;
  font-size: 14px;
  color: #fff;
  display: inline-block;
  line-height: 30px;
  //
}

.left_Statistics_center {
  width: 100%;
  // height: 85%;
  // height: 200px;
  background: url('../assets/newimg/newhome/框.png') no-repeat;
  background-size: 100% 100%;
  padding: 10px;
  box-sizing: border-box;
  position: relative;
}

.left_Statistics_center1 {
  width: 100%;
  height: 200px;
  // height: 200px;
  background: url('../assets/newimg/newhome/框.png') no-repeat;
  background-size: 100% 100%;
  padding: 10px;
  box-sizing: border-box;
  position: relative;
}

.left_Statistics_center2 {
  width: 100%;
  height: 240px;
  // height: 200px;
  background: url('../assets/newimg/newhome/框.png') no-repeat;
  background-size: 100% 100%;
  padding: 10px;
  box-sizing: border-box;
  position: relative;
}
.left_Statistics_center2 {
  width: 100%;
  height: 240px;
  // height: 200px;
  background: url('../assets/newimg/newhome/框.png') no-repeat;
  background-size: 100% 100%;
  padding: 10px;
  box-sizing: border-box;
  position: relative;
}
.left_Statistics_center3 {
  width: 100%;
  height: 200px;
  // height: 200px;
  background: url('../assets/newimg/newhome/框.png') no-repeat;
  background-size: 100% 100%;
  padding: 10px;
  box-sizing: border-box;
  position: relative;
}
.left_Statistics_centerg {
  width: 100%;
  height: 85%;
  // height: 240px;
  background: url('../assets/newimg/newhome/框.png') no-repeat;
  background-size: 100% 100%;
  padding: 10px;
  box-sizing: border-box;
  position: relative;
}
.left_Statistics_center_first,
.left_Statistics_center_first1 {
  width: 100%;
  height: 50%;
}
.left_Statistics_center_first_one,
.left_Statistics_center_first_two {
  float: left;
  width: 50%;
  height: 100%;
  box-sizing: border-box;
  display: flex;
  justify-content: center;
  align-items: center;
  // border: 1px solid red;
}

// .te {
//   padding-top: 10px;
// }
.left_Statistics_center_first_img,
.left_Statistics_center_first_type {
  float: left;
}
.left_Statistics_center_first_img {
  // width: 50px;
  // height: 50px;
  margin-top: 80px;
  width: 75px;
  height: 75px;
}

.left_Statistics_center_first_img_one {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  border: 2px solid rgb(17, 62, 110);
  background-color: rgba(17, 62, 110, 0.5);
  position: relative;
  animation: myfour 3s linear infinite;
  animation-iteration-count: infinite;
}
.left_Statistics_center_first_img_oneone {
  // 改之前
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 80%;
  height: 80%;
  border-radius: 50%;
  border: 2px solid #0887b7;
  background-color: #112c48;
  // 改之后
  // width: 90%;
  // height: 90%;
  // margin-top: 20px;
}
.left_Statistics_center_first_img_one img {
  width: 50%;
  height: 50%;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.left_Statistics_center_first_img_two {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  border: 2px solid #03326b;
  background-color: rgba(24, 47, 82, 0.5);
  position: relative;
  animation: myfour 3s linear infinite;
  animation-iteration-count: infinite;
}
.left_Statistics_center_first_img_twotwo {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 80%;
  height: 80%;
  border-radius: 50%;
  border: 2px solid #0c458a;
  background-color: #112c48;
}
.left_Statistics_center_first_img_two img {
  width: 50%;
  height: 50%;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.left_Statistics_center_first_img_three {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  border: 2px solid rgb(62, 59, 71);
  background-color: rgba(62, 59, 71, 0.5);
  position: relative;
  animation: myfour 3s linear infinite;
  animation-iteration-count: infinite;
}
.left_Statistics_center_first_img_threethree {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 80%;
  height: 80%;
  border-radius: 50%;
  border: 2px solid #e1727c;
  background-color: #112c48;
}
.left_Statistics_center_first_img_three img {
  width: 50%;
  height: 50%;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.left_Statistics_center_first_img_four {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  border: 2px solid rgb(63, 71, 69);
  background-color: rgba(63, 71, 69, 0.5);
  position: relative;
  animation: myfour 3s linear infinite;
  animation-iteration-count: infinite;
}
.left_Statistics_center_first_img_fourfour {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  width: 80%;
  height: 80%;
  border-radius: 50%;
  border: 2px solid #bc8947;
  background-color: #112c48;
}
.left_Statistics_center_first_img_four img {
  width: 50%;
  height: 50%;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

// .left_Statistics_center_first_img_zhong{
//   position: absolute;
//   width: 20px;
//   height: 20px;
//   top: 50%;
//     left: 50%;
//     transform: translate(-50%, -50%)
// }
// .left_Statistics_center_first_img_zhong img{
//    width: 100%;
//   height: 100%;
// }
.left_Statistics_center_first_type {
  margin-left: 10px;
  margin-top: 10px;
  width: 120px;
  height: 50px;
}
.left_Statistics_center_first_type_wenzi {
  // 改之前
  // font-size: 14px;
  color: #fcfffe;
  // letter-spacing: 1px;
  // margin-bottom: 10px;
  font-size: 20px;
  letter-spacing: 2px;
  margin:27px 0 22px 5px
}
.left_Statistics_center_first_type_num {
  margin-left: 5px;
  font-size: 20px;
}
.color {
  color: #00baff;
  font-weight: 500;
}
.color1 {
  color: #1c79f1;
  font-weight: 500;
}
.color2 {
  color: #ff817f;
  font-weight: 500;
}
.color3 {
  color: #faad50;
  font-weight: 500;
}
.p {
  font-size: 12px;
  color: #788c9f;
}
.di {
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 3px;
  background: url('../assets/newimg/newhome/框底的点.png') no-repeat;
  background-size: 100% 100%;
}
.center1_map {
  width: 1027px;
  height: 100%;
  box-sizing: border-box;
}
.nav {
  width: 1027px;
  height: 6%;
  margin: 0 auto;
  // border: 1px solid red;
  // background: url('../assets/newimg/newhome/形状 6.png') no-repeat;
  // background-size: 100% 100%;
  position: relative;
  margin-top: -5px;
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
.dianl {
  position: absolute;
  width: 50px /* 50/16 */;
  height: 32px;
  cursor: pointer;
  left: 30px;
  top: 0;
}
.dianl img {
  width: 100% /* 50/16 */;
  height: 100%;
}
.dianr {
  position: absolute;
  width: 50px /* 50/16 */;
  height: 32px;
  cursor: pointer;
  right: 30px;
  top: 0;
}
.dianr img {
  width: 100% /* 50/16 */;
  height: 100%;
}
.menu_xuanzhong {
  position: absolute;
  width: 22px /* 50/16 */;
  height: 20px;
  bottom: 0;
  left: 46.5%;
}
.menu_xuanzhong img {
  width: 100%;
  height: 100%;
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
  border: 3px solid #0d6fd8;
  margin-right: 8px;
  box-shadow: 0 0 10px #1768be inset, 0 0 0px #b5c5d4;
  //  background-color: ;
  // border: 1px soild red;
  // font-weight: 400;
  // font-family: 'heiti';
  // font-family: '黑体';

  // background-image: linear-gradient(0deg, #0ae5f5 0%, #0ef0a1 100%);
  // -webkit-background-clip: text;
  // -webkit-text-fill-color: transparent;
  // letter-spacing: 2px;
  // text-shadow: 5px 0px 10px rgba(119, 168, 224, 0.5);

  // margin-right: 2px;
  // padding: 0 10px;
}

.nav_li .daohang3 li {
  transform: skewX(45deg);
  // border-bottom-color:#0d6fd8;
}
.nav_li .daohang3 li span {
  display: inline-block;
  transform: skewX(-45deg);
}
.nav_li .daohang3 li:hover {
  // background-image: linear-gradient(0deg, #0a35f5 0%, #07f8e4 100%);
  // -webkit-background-clip: text;
  // -webkit-text-fill-color: transparent;
  color: #fff;
  border-bottom-color: #07e7e6;
  //  text-shadow: 0 0 20px rgba(189, 207, 228, 0.5);
}

.nav_li .daohang2 li {
  float: left;
  list-style-type: none;
  text-align: center;
  // font-size: 1.25rem /* 20/16 */;
  // color: #fff;
  font-weight: 400;
  // padding: 0  ;
  // padding-top: 5px;
  // padding-right: 1.1rem;
  // padding-left: 1.4rem;
  // box-sizing: border-box;
  // width: 5.6rem /* 71/16 */ /* 44.5/16 */;
  width: 212px;
  height: 45px;
  // padding-left: 0.7rem;
  // box-sizing: border-box;
}
.nav_li .daohang1 li {
  float: left;
  list-style-type: none;
  text-align: center;
  // font-size: 1.25rem /* 20/16 */;
  // color: #fff;
  font-weight: 400;
  // padding: 0  ;
  // padding-top: 5px;
  // padding-right: 1.1rem;
  // padding-left: 1.4rem;
  // box-sizing: border-box;
  // width: 5.6rem /* 71/16 */ /* 44.5/16 */;
  width: 100%;
  height: 45px;
  // padding-left: 0.7rem;
  // box-sizing: border-box;
}
.three {
  font-size: 22px /* 20/16 */ /* 18/16 */;
  color: #dfe0f7;
  font-weight: 500;
}
.three:hover {
  color: #fff;
  text-shadow: 1px 1px 20px #0068c4;
}

.threes {
  font-size: 20px /* 16/16 */;
  color: #fff;
  font-weight: 500;
}
.four {
  font-size: 20px /* 16/16 */;
  font-weight: 500;
  background-image: linear-gradient(to left, #d6e1e5 0%, #51626d 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  // pointer-events: none;
}
.five {
  font-size: 20px /* 16/16 */;
  font-weight: 500;
  color: #fff;
  border-bottom-color: #07e7e6 !important;
  //  background-image: linear-gradient(0deg, #0a35f5 0%, #07f8e4 100%);
  // -webkit-background-clip: text;
  // -webkit-text-fill-color: transparent;
  // pointer-events: none;
}
.six {
  font-size: 20px /* 16/16 */;
  font-weight: 500;
  color: #b5b5d4;
  // text-shadow: 0 0 20px #0079ff;
  // background-image: linear-gradient(to right, #d6e1e5 0%, #51626d 100%);
  // -webkit-background-clip: text;
  // -webkit-text-fill-color: transparent;
  // pointer-events: none;
}
.six:hover {
  color: #fff;
  border-bottom-color: #07e7e6 !important;
}
.center1_right {
  width: 445px;
  padding-right: 30px;
  padding-top: 38px;
  height: 100%;
  box-sizing: border-box;
  // border: 1px solid red;
}
.left_type {
  width: 100%;
  // height: 29%;
  flex: 1;
  margin-bottom: 26px;
}
.left_trend {
  width: 100%;
  // height: 37%;
  flex: 1;
  // margin-bottom: 26px;
}
.tishititle {
  display: flex;
  justify-content: center;
  // align-items: center;
  width: 100%;
  height: 13%;
  margin-top: 22px;
}
.faxian,
.fanzhi,
.lanjie {
  float: left;
  width: 200px;
  height: 90px;
  //  border: 1px solid blue;
  margin-left: 24px;
  // background: ;
}
.faxian {
  // margin-left: 88px !important;
  background: url('../assets/newimg/newhome/框 实时.png') no-repeat;
  background-size: 100% 100%;
  text-align: center;
  position: relative;
}
.beij {
  position: absolute;
  width: 40px;
  height: 40px;
  top: -14px;
  left: -20px;
  background: url('../assets/newimg/newhome/圆 装饰.png') no-repeat;
  background-size: 100% 100%;
  animation: mysecond 5s linear infinite;
  animation-iteration-count: infinite;
}
.fanzhi {
  background: url('../assets/newimg/newhome/框实时2.png') no-repeat;
  background-size: 100% 100%;
  text-align: center;
}
.lanjie {
  background: url('../assets/newimg/newhome/框实时3.png') no-repeat;
  background-size: 100% 100%;
  position: relative;
  text-align: center;
}
.beij1 {
  position: absolute;
  width: 40px;
  height: 40px;
  top: -14px;
  right: -20px;
  background: url('../assets/newimg/newhome/圆 装饰.png') no-repeat;
  background-size: 100% 100%;
  animation: mythree 5s linear infinite;
  animation-iteration-count: infinite;
}
.jin {
  width: 100%;
  height: 40px;
  font-size: 20px;
  padding-top: 10px;
  box-sizing: border-box;
  color: #fff;
}
.jin_num {
  width: 100%;
  height: 40px;
  font-size: 34px !important;
  line-height: 40px;
  color: #63b4ff;
  text-shadow: 0px 2px 2px black;
  font-family: 'Arial';
}
.jin_num3 {
  width: 100%;
  height: 40px;
  font-size: 34px !important;
  line-height: 40px;
  color: #eaa14d;
  text-shadow: 0px 2px 2px black;
}
.jin_num4 {
  width: 100%;
  height: 40px;
  font-size: 34px !important;
  line-height: 40px;
  color: #4dea67;
  text-shadow: 0px 2px 2px black;
}
.jin_num2 {
  width: 100%;
  height: 40px;
  font-size: 34px !important;
  line-height: 40px;
  color: #fd818a;
  text-shadow: 0px 2px 2px black;
}
.jin_num1 {
  font-style: normal;
  font-size: 18px;
  color: #5b749d;
  text-shadow: none;
}
.map_cd {
  height: 75%;
  width: 100%;
  // border: 1px solid #ccc;
  padding: 0 20px;
  box-sizing: border-box;
  position: relative;
}
// 地图
// 地图
// 地图
.left_huanbi {
  width: 100%;
  // height: 28%;
  flex: 1;
  margin-bottom: 1%;
}
.left_Statistics_center_one {
  width: 390px;
  height: 45%;
  // margin:0  16px;
  background-image: linear-gradient(120deg, #123359, #041f39);
  padding: 10px;
  box-sizing: border-box;
  display: flex;
  justify-content: center;
  align-items: center;
}
.left_Statistics_center_two {
  width: 390px;
  height: 45%;
  margin-top: 16px;
  background-image: linear-gradient(120deg, #23324c, #061e39);
  padding: 10px;
  box-sizing: border-box;
  display: flex;
  justify-content: center;
  align-items: center;
}
.left_Statistics_center_img,
.left_Statistics_center_title,
.left_Statistics_center_line,
.left_Statistics_center_bilv {
  float: left;
}
.left_Statistics_center_img {
  position: relative;
}
.img1 {
  position: absolute;
  bottom: 24%;
  left: -3%;
  width: 100%;
  animation: myfive 0.5s linear infinite;
  animation-iteration-count: infinite;
}
.left_Statistics_center_img .imgone {
  width: 68px;
  height: 58px;
  margin-top: 4px;
}
.left_Statistics_center_title {
  width: 145px;
  height: 68px;
  padding: 5px 0 0 15px;
  box-sizing: border-box;
}
.titleone {
  height: 14px;
  width: 100%;
  font-size: 16px;
  color: #fff;
  margin-bottom: 10px;
}
.titlenum {
  height: 30px;
  width: 100%;
  font-size: 26px;
  font-weight: bold;
  background-image: linear-gradient(180deg, #98c6ff 0%, #027dff 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
.titlenum1 {
  height: 30px;
  width: 100%;
  font-size: 26px;
  font-weight: bold;
  background-image: linear-gradient(180deg, #d1d5d9 0%, #ff8281 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.left_Statistics_center_line {
  width: 3px;
  height: 71px;
  background: url('../assets/newimg/newhome/分割线.png') no-repeat;
  background-size: 100% 100%;
}
.left_Statistics_center_bilv {
  width: 150px;
  height: 68px;
  padding: 5px 0 0 20px;
  box-sizing: border-box;
}
.titleone1 {
  height: 14px;
  width: 100%;
  font-size: 16px;
  color: #fff;
  margin-bottom: 14px;
}

.titlenum21 {
  height: 30px;
  width: 100%;
  font-size: 22px;
  font-weight: bold;
  background-image: linear-gradient(180deg, #027dff 0%, #98c6ff 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
.titleone2 {
  height: 14px;
  width: 100%;
  font-size: 16px;
  color: #fff;
  margin-bottom: 12px;
}

.titlenum22 {
  height: 30px;
  width: 100%;
  font-size: 22px;
  font-weight: bold;
  background-image: linear-gradient(180deg, #d1d5d9 0%, #ff8281 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
.left_region {
  width: 100%;
  height: 39%;
}

.btm_one {
  height: 14% /* 76/16 */ /* 70/16 */ /* 76/16 */ /* 58/16 */;
  width: 100%;
  // border: 1px solid #ccc;
  // padding-top: 10px /* 10/16 */;
  box-sizing: border-box;
  margin-top: 13px;
}
.btm_one_top {
  // width: 2.25rem /* 36/16 */;
  width: 100%;
  height: 20px /* 24/16 */ /* 20/16 */;
  // border: 1px solid red;
  margin-bottom: 8px;
  box-sizing: border-box;
}
.btm_one_top img {
  float: left;
  width: 36px /* 36/16 */;
  height: 18px /* 20/16 */;
}
.btm_one_top .name {
  float: left;
  color: #fff;
  font-size: 16px /* 18/16 */;
  margin: 0 0 0 /* 5/16 */ /* 10/16 */ 10px /* 20/16 */;
  box-sizing: border-box;
  // margin: 0 0 .25rem /* 4/16 */ /* 2/16 */ 1.25rem /* 20/16 */;
  display: block;
  letter-spacing: 1px;
}
.right_num1 {
  float: right;
  font-size: 22px /* 22/16 */;
  color: #e97c75;
}
.right_num2 {
  float: right;
  font-size: 22px /* 22/16 */;
  color: #ebd50e;
}
.right_num3 {
  float: right;
  font-size: 22px /* 22/16 */;
  color: #9ce70f;
}
.right_num4 {
  float: right;
  font-size: 22px /* 22/16 */;
  color: #0fddfd;
}
.right_num5 {
  float: right;
  font-size: 22px /* 22/16 */;
  color: #0e7ced;
}
.btm_jdt {
  width: 100%;
  height: 22px /* 34/16 */;
}
.btm_zhuzhuang {
  width: 100%;
  height: 6px /* 6/16 */;
  background-color: #00294a;
  margin-top: 2px /* 10/16 */;
  position: relative;
}
.btm_color1 {
  position: absolute;
  top: 0;
  left: 0;
  // width: 80% /* 6/16 */;
  height: 100%;
  background: linear-gradient(to right, #ed3f40, #ff7e7e);
}
.btm_color2 {
  position: absolute;
  top: 0;
  left: 0;
  // width: 80% /* 6/16 */;
  height: 100%;
  background: linear-gradient(to right, #ff9119, #fbec74);
}
.btm_color3 {
  position: absolute;
  top: 0;
  left: 0;
  // width: 80% /* 6/16 */;
  height: 100%;
  background: linear-gradient(to right, #fbff00, #8dc4fd);
}

.btm_color4 {
  position: absolute;
  top: 0;
  left: 0;
  // width: 80% /* 6/16 */;
  height: 100%;
  background: linear-gradient(to right, #47bdbd, #05e0fd);
}

.btm_color5 {
  position: absolute;
  top: 0;
  left: 0;
  // width: 80% /* 6/16 */;
  height: 100%;
  background: linear-gradient(to right, #0db7dd, #4080cf);
}

.cir1 {
  position: absolute;
  top: -9px;
  // left: -20px;
  width: 20px /* 20/16 */;
  height: 20px;
  border: 2px solid rgb(233, 124, 117);
  border-radius: 50%;
  background: rgba(233, 124, 117, 0.4);
}
.cir2 {
  position: absolute;
  top: -9px;
  // left: -20px;
  width: 20px /* 20/16 */;
  height: 20px;
  border: 2px solid rgb(246, 243, 121);
  border-radius: 50%;
  background: rgba(246, 243, 121, 0.3);
}
.cir3 {
  position: absolute;
  top: -9px;
  // left: -20px;
  width: 20px /* 20/16 */;
  height: 20px;
  border: 2px solid rgb(88, 159, 234);
  border-radius: 50%;
  background: rgba(88, 159, 234, 0.3);
}
.cir4 {
  position: absolute;
  top: -9px;
  // left: -20px;
  width: 20px /* 20/16 */;
  height: 20px;
  border: 2px solid rgb(0, 156, 186);
  border-radius: 50%;
  background: rgba(0, 156, 186, 0.3);
}
.cir5 {
  position: absolute;
  top: -9px;
  // left: -20px;
  width: 20px /* 20/16 */;
  height: 20px;
  border: 2px solid rgb(15, 221, 253);
  border-radius: 50%;
  background: rgba(15, 221, 253, 0.3);
}

.cir_li {
  position: absolute;
  top: 5px /* 5/16 */ /* 4/16 */;
  left: 5px /* 5/16 */ /* 4/16 */;
  width: 10px /* 10/16 */;
  height: 10px /* 10/16 */;
  background-color: #fff;
  border-radius: 50%;
}
.left_information {
  // height: 26%;
  flex: 1;
  width: 100%;
}
.youwenzi {
  position: absolute;
  width: 60%;
  height: 30px;
  top: 80%;
  right: 5%;
  transform: translateY(-50%);
}
.qiehuan li {
  float: right;
  list-style: none;
  padding-right: 10px;
  font-size: 12px;
  color: #fff;
  cursor: pointer;
  letter-spacing: 1px;
  padding-top: 2px;
}

.Active {
  background-image: linear-gradient(180deg, #02dafc 0%, #0099f0 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: 700;
}
.xinix {
  overflow: hidden;
}
.title_top2 {
  width: 100%;
  // height: 30px;
  height: 17%;
  background-color: transparent;
  border: 1px solid #1f436c;
  padding: 1% 0;
}
.title_top2 span:nth-child(1) {
  display: inline-block;
  width: 27%;
  line-height: 17%;
  color: #80a2c0;
  text-align: center;
  font-size: 14px;
}
.title_top2 span:nth-child(2) {
  display: inline-block;
  width: 38%;
  line-height: 17%;
  color: #80a2c0;
  text-align: center;
  font-size: 14px;
  margin-left: 1%;
}
.title_top2 span:nth-child(3) {
  display: inline-block;
  width: 29%;
  line-height: 17%;
  color: #80a2c0;
  text-align: center;
  font-size: 14px;
  margin-left: 2%;
}

.title_top {
  width: 100%;
  // height: 30px;
  height: 17%;
  background-color: transparent;
  border: 1px solid #1f436c;
  padding: 1% 0;
}
.title_top span:nth-child(1) {
  display: inline-block;
  width: 35%;
  line-height: 17%;
  color: #80a2c0;
  text-align: center;
  font-size: 14px;
}
.title_top span:nth-child(2) {
  display: inline-block;
  width: 17%;
  line-height: 17%;
  color: #80a2c0;
  text-align: center;
  font-size: 14px;
  margin-left: 1%;
}
.title_top span:nth-child(3) {
  display: inline-block;
  width: 19%;
  line-height: 17%;
  color: #80a2c0;
  text-align: center;
  font-size: 14px;
  margin-left: 2%;
}
.title_top span:nth-child(4) {
  display: inline-block;
  width: 30%;
  line-height: 17%;
  color: #80a2c0;
  text-align: center;
  font-size: 14px;
  margin-left: 2%;
}
.seamless-warp1 {
  height: 170px /* 240/16 */;
  overflow: hidden;
}
.seamless-warp1 li {
  height: 28px /* 30/16 */;
  line-height: 28px /* 30/16 */;
  width: 100%;
  border: 1px /* 1/16 */ solid #1f436c;
  margin-top: 10px /* 10/16 */ /* 5/16 */ /* 3/16 */;
  box-sizing: border-box;
  // background-color: rgba(255, 255, 255, .1);
}
.seamless-warp1 span:nth-child(1) {
  display: inline-block;
  width: 28%;
  height: 100%;
  text-align: center;
  font-size: 10px;
  -webkit-transform: scal(0.83);
  overflow: hidden;
}
.seamless-warp1 span:nth-child(2) {
  display: inline-block;
  width: 39%;
  margin-left: 1%;
  height: 100%;
  text-align: center;
  font-size: 10px;
  -webkit-transform: scal(0.83);
  overflow: hidden;
}
.seamless-warp1 span:nth-child(3) {
  display: inline-block;
  width: 29%;
  margin-left: 3%;
  height: 100%;
  text-align: center;
  font-size: 10px;
  -webkit-transform: scal(0.83);
  overflow: hidden;
}
// .seamless-warp1 span:nth-child(4) {
//   display: inline-block;
//   width: 30%;
//   margin-left: 2%;
//   height: 100%;
//   text-align: center;
//   font-size: 10px;
//   -webkit-transform: scal(0.83);
//   overflow: hidden;
// }
#dianre {
  display: inline-block;
  width: 5px;
  height: 5px;
  border-radius: 2.5px;
  background-color: #34de8e;
  margin-right: 5px;
  margin-bottom: 2px;
}
#dianre1 {
  display: inline-block;
  width: 5px;
  height: 5px;
  border-radius: 2.5px;
  background-color: red;
  margin-right: 5px;
  margin-bottom: 2px;
}
#dianre2 {
  display: inline-block;
  width: 5px;
  height: 5px;
  border-radius: 2.5px;
  background-color: yellow;
  margin-right: 5px;
  margin-bottom: 2px;
}

.title_top1 {
  width: 100%;
  height: 17%;
  background-color: transparent;
  border: 1px solid #1f436c;
  padding: 1% 0;
}
.title_top1 span:nth-child(1) {
  display: inline-block;
  width: 25%;
  line-height: 17%;
  color: #80a2c0;
  text-align: center;
  font-size: 14px;
}
.title_top1 span:nth-child(2) {
  display: inline-block;
  width: 16%;
  line-height: 17%;
  color: #80a2c0;
  text-align: center;
  font-size: 14px;
  margin-left: 1%;
}
.title_top1 span:nth-child(3) {
  display: inline-block;
  width: 25%;
  line-height: 17%;
  color: #80a2c0;
  text-align: center;
  font-size: 14px;
  margin-left: 3%;
}
.title_top1 span:nth-child(4) {
  display: inline-block;
  width: 20%;
  line-height: 17%;
  color: #80a2c0;
  text-align: center;
  font-size: 14px;
  margin-left: 2%;
}
.seamless-warp {
  height: 170px /* 240/16 */;
  overflow: hidden;
}

.seamless-warp li {
  height: 28px /* 30/16 */;
  line-height: 28px /* 30/16 */;
  width: 100%;
  border: 1px /* 1/16 */ solid #1f436c;
  margin-top: 10px /* 10/16 */ /* 5/16 */ /* 3/16 */;
  box-sizing: border-box;
  // background-color: rgba(255, 255, 255, .1);
}
.seamless-warp span:nth-child(1) {
  display: inline-block;
  width: 25%;
  height: 100%;
  text-align: center;
  font-size: 10px;
  -webkit-transform: scal(0.83);
  overflow: hidden;
}
.seamless-warp span:nth-child(2) {
  display: inline-block;
  width: 12%;
  margin-left: 1%;
  height: 100%;
  text-align: center;
  font-size: 10px;
  -webkit-transform: scal(0.83);
  overflow: hidden;
}
.seamless-warp span:nth-child(3) {
  display: inline-block;
  width: 30%;
  margin-left: 3%;
  height: 100%;
  text-align: center;
  font-size: 10px;
  -webkit-transform: scal(0.83);
  overflow: hidden;
}
.seamless-warp span:nth-child(4) {
  display: inline-block;
  width: 20%;
  margin-left: 2%;
  height: 100%;
  text-align: center;
  font-size: 10px;
  -webkit-transform: scal(0.83);
  overflow: hidden;
}

.legend-box {
  height: 115px /* 115/16 */;
  position: absolute;
  bottom: 5px /* 20/16 */;
  right: 10px /* 10/16 */;
  padding: 10px;
  border: 3px solid #0d375a;
}

.legend-box .titleent {
  color: #fff;
  font-size: 12px /* 12/16 */;
  font-weight: 500;
  text-align: center;
  width: 100%;
  height: 20px /* 20/16 */;
  line-height: 20px;
}

.legend-box .down {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -ms-flex-pack: distribute;
  justify-content: space-around;
}

.legend-box .down .area-box {
  width: 60px /* 60/16 */;
  height: 85px /* 85/16 */;
}

.legend-box .down .area-box .area-lengend {
  width: 100%;
  height: 100%;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -ms-flex-pack: distribute;
  justify-content: space-around;
}

.legend-box .down .area-box .area-lengend .left-color {
  width: 20px /* 20/16 */;
  height: 100%;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-pack: center;
  -ms-flex-pack: center;
  justify-content: center;
  border-top: 1px solid #fff;
  border-bottom: 1px solid #fff;
}

.legend-box .down .area-box .area-lengend .left-color .inside {
  width: 10px /* 10/16 */;
  height: 100%;
  // background: -webkit-gradient(
  //   linear,
  //   left top,
  //   left bottom,
  //   from(#da1222),
  //   color-stop(20%, #f4652a),
  //   to(#00c1e0)
  // );
  background: linear-gradient(
    180deg,
    #f12f49,
    #ed3f40 15%,
    #ff9119 25%,
    #c2c50f 50%,
    #47bdbd 65%,
    #4080cf 85%,
    #287fe9
  );
}

.legend-box .down .area-box .area-lengend .right-text {
  width: 60px /* 60/16 */;
  height: 100%;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  -ms-flex-direction: column;
  flex-direction: column;
  -webkit-box-pack: justify;
  -ms-flex-pack: justify;
  justify-content: space-between;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
}

.legend-box .down .area-box .area-lengend .right-text .com {
  font-size: 10px /* 10/16 */ /* 12/16 */ /* 10/16 */ /* 12/16 */;
  font-family: Source Han Sans CN;
  font-weight: 500;
  color: #fff;
  // -webkit-transform: scale(0.8);
}
.com {
  //  font-size:.625rem /* 10/16 */;
  -webkit-transform: scale(0.9);
}
.legend-box .down .column-box {
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  -ms-flex-direction: column;
  flex-direction: column;
  -webkit-box-pack: justify;
  -ms-flex-pack: justify;
  justify-content: space-between;
  display: flex;
  -webkit-box-align: center;
  align-items: center;
}

.legend-box .down .column-box .col-item {
  width: 100%;
  display: flex;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
}

.legend-box .down .column-box .col-item .item-icon {
  width: 10px /* 10/16 */;
  height: 10px;
  border-radius: 50%;
  margin-right: 5px /* 5/16 */;
}

.legend-box .down .column-box .col-item .item-name {
  font-size: 10px /* 10/16 */;
  font-family: Source Han Sans CN;
  font-weight: 500;
  color: #feffff;
}
.ne {
  height: 80%;
}

.fanhui {
  position: absolute;
  left: 0.5%;
  top: 15%;
  cursor: pointer;
}
.jinzhi1 {
  font-size: 20px /* 16/16 */;
  font-weight: 500;
  color: #b5b5d4;
  cursor: not-allowed !important;
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