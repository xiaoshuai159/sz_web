<template>
  <div class="app dp" v-loading="loading" element-loading-text="拼命加载中" element-loading-spinner="el-icon-loading"
    element-loading-background="rgba(0, 0, 0, 0.8)">

    <!-- 头部 -->
    <div class="top1">
      <div class="top1_title">
        <div class="title">深圳市反诈技术支撑平台</div>
        <div class="topguanli">
          <div class="role">
            <img :src="require('../assets/img/shouye/guanliyuan.png')" alt="" />
            <el-dropdown class="user-name" trigger="click" @command="handleCommand">
              <span class="el-dropdown-link" style="position: relative;top:2px">
                {{ username }}
                <i class="el-icon-caret-bottom"></i>
              </span>
              <template #dropdown>
                <el-dropdown-menu>
                  <el-dropdown-item command="upload">修改密码</el-dropdown-item>
                  <el-dropdown-item divided command="loginout">退出登录</el-dropdown-item>
                </el-dropdown-menu>
              </template>
            </el-dropdown>
          </div>
        </div>

        <div class="fanhui">
          {{ curDate }}
        </div>

      </div>
    </div>



    <Navlist></Navlist>
   


    <div class="center2">
      <div class="center2_left">
        <div class="gsh3"><span>·</span> 处置统计</div>
        <br>
        <div class="center2_left_test1">总处置数量</div>
        <div style="display: flex; font-size: 0.20rem;">
          <div class="picture_test1">{{ total[0] }}</div>
          <div class="picture_test2">{{ total[1] }}</div>
          <div class="picture_test2">{{ total[2] }}</div>
          <div class="picture_test2">{{ total[3] }}</div>
          <div class="picture_test2">{{ total[4] }}</div>
          <div class="picture_test2">{{ total[5] }}</div>
          <div class="picture_test2">{{ total[6] }}</div>
        </div>
        <div class="center2_left_test2">昨日处置数量</div>
        <div style="display: flex; font-size: 0.20rem; color:#fbdb64">
          <div class="picture_test3">{{ cur[0] }}</div>
          <div class="picture_test4">{{ cur[1] }}</div>
          <div class="picture_test4">{{ cur[2] }}</div>
          <div class="picture_test4">{{ cur[3] }}</div>
        </div>
        <div class="center2_left_test3">日环比 {{ compareNum }}</div>
        <img v-if="upOrDe==2" class="picture_test12" src="../assets/img/shouye/下降.png">
        <img v-else-if="upOrDe==1" class="picture_test12" src="../assets/img/shouye/上升.png">
      </div>

      <div class="center2_center">
        <div class="gsh3"><span>·</span> 7日处置高危类型TOP5</div>
        <div ref="mypieChart" class="add1"></div>
      </div>

      <div class="center2_right">
        <div class="gsh3"><span>·</span> 7日处置量趋势</div>
        <div style="width: 100%;height: 100%;transform: translate(0, -40px);">
          <div ref="mybarChart" style="width:100%; height: 100%;"></div>
        </div>
      </div>
    </div>




    <div class="center3">
      <div style="float: right;">
          <el-date-picker
            style="transform: translate(-0.25rem,0.39rem); width: 200px; z-index: 99;"
            v-model="dateRange"
            type="daterange"
            range-separator="-"
            start-placeholder="开始日期"
            end-placeholder="结束日期"
            value-format="yyyy-MM-dd"
            :clearable = 'false'
            size="mini"
          >
          </el-date-picker>
        </div>
      <div class="center3_one">
        <div class="gsh3"><span>·</span> 处置涉诈类型分析</div>
        <div class="pos">
          <div ><input type="checkbox" class="box" v-model="selectAll" @change="updateSelectAll" :id="'selectAll'"><label :for="'selectAll'" class="pos1" style="cursor: pointer;">全部类型</label></div>
          <div v-for="(item, index) in items" :key="index" style="cursor: pointer;">
            <input type="checkbox" :id="'checkbox_'+index" class="box" v-model="selectedItems[index]">
            <label :for="'checkbox_'+index" style="cursor: pointer;">{{ item }}</label><br>
          </div>
        </div>
        <div style="position: fixed; display: inline-block; transform: translate(0.2rem,0); z-index: 99; color: aliceblue;">
          <button :class="{ 'selected': mode === 'week' }" @click="mode = 'week'" class="transparent-button">星期</button>
          /
          <button :class="{ 'selected': mode === 'month' }" @click="mode = 'month'" class="transparent-button">月</button>
        </div>
        
            <!-- :change="dataCreate_change(dateRange)" -->
        <div style="display: inline-block; margin-left: 15px; width: 88%; height: 71%;">
          <div ref="mylineChart" style="width:100%; height: 100%;"></div>
        </div>
      </div>
    </div>

    <div class="myfooter"></div>

    <el-dialog title="修改密码" :visible.sync="dialog" width="30%" :before-close="handleClose1" class="dialogInfo"
      :close-on-click-modal="false">
      <el-form :rules="rules" ref="newdomainSimpleVo" label-width="80px" :inline="true" :model="newdomainSimpleVo"
        class="demo-form-inline search_select_form" size="mini">
        <el-form-item label="原密码" prop="oldpwd">
          <el-input v-model="newdomainSimpleVo.oldpwd" placeholder="请输入原密码"></el-input>
        </el-form-item>
        <el-form-item label="原密码" prop="oldpwd" style="visibility: hidden" v-if="yinc">
          <el-input v-model="newdomainSimpleVo.oldpwd" placeholder="请输入原密码"></el-input>
        </el-form-item>
        <el-form-item label="新密码" prop="xinpwd">
          <el-input v-model="newdomainSimpleVo.xinpwd" placeholder="请输入新密码"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="submitForm('newdomainSimpleVo')" type="primary" size="mini" class="el-button-xitongerr">修
          改</el-button>
        <el-button class="el-button-xitongup" type="primary" @click="err" size="mini">取 消</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import dayjs from 'dayjs'
import Navlist from '@/components/hearderdongtainav.vue'
import echarts from 'echarts'
export default {
  // components: { getDiscover },
  components: {
    Navlist: Navlist,
  },
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
    return {
      selectAll: false,
      selectedItems: [],
      items: [
        '虚假贷款',
        '虚假电商',
        '虚假理财',
        '杀猪盘赌',
        '下载链接',
        '冒充客服',
        '刷单返利',
        '网络游戏',
        '网络婚恋'
      ],
      mode: 'month',
      dateRange:[
          dayjs().subtract(1, 'month').format('YYYY-MM-DD'),
          dayjs().format('YYYY-MM-DD')
        ],
      total:[0,0,0,0,0,0,0],
      cur:[0,0,0,0],
      compareNum: '-',
      upOrDe:1,
      curDate: dayjs().format("YYYY年MM月DD日  HH时mm分ss秒"),
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
          }
        ],

      },
      //居中导航
      daohang: [
        // 7.4 新增测试数据
        {
          name: '首页',
          path: '/shouye',
        }, {
          name: '发现',
          path: '/gitDiscoverca',
        }, {
          name: '处置',
          path: '/disposePage',
        }, {
          name: '警情',
          path: '/getWarningfourg'
        }, {
          name: '特办',
          path: '/specialPage'
        }, {
          name: '关联',
          path: '/relevancePage'
        },
        {
          name: '管理',
          path: '/findRole',
        },
      ],
      //头部loading
      // loading: true, // 7.4 测试暂时关闭
      loading: false,
      pid: [],
      Newname: [],
      Newname1: [],
      active: '',
      name: 'wu',
      dialog: false,
      newdomainSimpleVo: {
        oldpwd: '',
        xinpwd: ''
      },
      yinc: false,
      // loading1: false,
    }
  },
  computed: {
    username() {
      let username = JSON.parse(window.sessionStorage.getItem('one'))
      return username ? username : this.name
    },
  },
  created() {
    this.getDisposeNum()  // 获取处置统计
    this.getfraudTop5()
    this.getDisposeTrend()
   
  },
  mounted() {
    this.startTimer();
    // this.drawpieChart()
    // this.drawbarChart()
    this.drawlineChart()
  },
  methods: {
    startTimer()
    {
      setInterval(() => {
      this.curDate=dayjs().format("YYYY年MM月DD日  HH时mm分ss秒")
    }, 1000);

    },
    updateSelectAll() {
      if (this.selectAll) {
        this.selectedItems = Array(this.items.length).fill(true);
      } else {
        this.selectedItems = [];
      }
    },
    async getfraudTop5(){
      const {data:res} = await this.$http.get('/statistics/block/top5fraud')
      if(res.code == 200){
        const dataArr = res.data
        const convertedData = dataArr.map((item, index) => {
          const value = item.cnt;
          const name = `${(parseFloat(item.percent) * 100).toFixed(2)}% ${item.fraudType}`;
          const color = ['#17acf0', '#ecd456', '#7d69ef', '#fab300', '#2dd3bd'][index % 5];
          return {
            value,
            name,
            itemStyle: { color },
            labelLine: { normal: { show: false } }
          };
        })
        this.drawpieChart(convertedData)
      }
    },
    async getDisposeTrend(){
      const {data:res} = await this.$http.get('/statistics/block/latestBlockCnt')
      if(res.code == 200){
        const dataArr = res.data
        const xAxisData = dataArr["警情"].map(item => item.day.substr(5));
        const seriesData = []
        let curMax, curInterval
        for (const key in dataArr) {
          if (Array.isArray(dataArr[key])) {
            const data = dataArr[key].map(item => item.cnt);
            curMax = Math.ceil(Math.max(...data)) 
            curInterval = Math.floor(curMax/5)
            seriesData.push({
              name: key,
              type: 'bar',
              barWidth: 10,
              data,
              itemStyle: {
                color: key === '警情' ? new echarts.graphic.LinearGradient(
                  0, 0, 0, 1,
                  [
                    { offset: 0, color: '#e6210e' },
                    { offset: 1, color: '#f6c14e' }
                  ]
                ):new echarts.graphic.LinearGradient(
                  0, 0, 0, 1,
                  [
                    { offset: 0, color: '#12cddb' },
                    { offset: 1, color: '#020d2a' }
                  ]
                )
              }
            });
          }
        }
        this.drawbarChart(xAxisData,seriesData,curMax,curInterval)
      }
    },
    async getFraudTypeTrend(){

      const {data:res} = await this.$http.get('/statistics/block/fraudTypeCntTrend')
    },
    async getDisposeNum(){
      const {data:res} = await this.$http.get('/statistics/block/intro')
      if(res.code == 200){
        let {qoq,todayCnt,totalCnt} = res.data
        // qoq = 0.23
        parseInt(totalCnt) > 9999999 ? this.total = [9,9,9,9,9,9,9] : this.splitNum(totalCnt, 7)
        parseInt(todayCnt) > 9999 ? this.cur = [9,9,9,9] : this.splitNum(todayCnt, 4)
        if(qoq == '-'){
          this.upOrDe = 3
          this.compareNum = '-'
        }else if(qoq > 0){
          this.upOrDe = 1
          this.compareNum = this.convertToPercentage(qoq)
        }else{
          this.upOrDe = 2
          this.compareNum = this.convertToPercentage(Math.abs(qoq))
        }
      }else{
        this.$message(res.message)
      }
    },
    convertToPercentage(number) {
      const percentage = (number * 100).toFixed(2);
      return `${percentage}%`;
    },
    splitNum(num, len){
      const digits = Array.from(String(num), Number);
      const paddedDigits = Array(len - digits.length).fill(0).concat(digits);
      if(len==7){
        this.total = paddedDigits;
      }else if(len==4){
        this.cur = paddedDigits
      } 
    },

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
      await this.$http.post('/user/logout')
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
        oldPwd: this.newdomainSimpleVo.oldpwd,
        newPwd: this.newdomainSimpleVo.xinpwd
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
      let that = this
      this.$nextTick(() => {
        that.$refs['newdomainSimpleVo'].clearValidate()
      })
      this.dialog = false
    },
    drawpieChart(dataArr) {
      let pie_qx = this.$refs.mypieChart
      let pieChart = this.$echarts.init(pie_qx)
      window.addEventListener('resize', function () {
      pieChart.resize()
      })
      pieChart.clear()
      pieChart.setOption(this.setOptionPie(dataArr)) // 待完善：记得销毁echarts和resize
    },
    setOptionPie(dataArr) {

      let option = {
        // toolbox: {
        //   show: true
        // },
        tooltip: {
          trigger: 'item'
        },
        series: [
          {
            type: 'pie',
            radius: ['12%', '70%'],
            center: ['50%', '45%'],
            roseType: 'area',
            itemStyle: {
              borderRadius: 8,
              color: '#ffffff'
            },
            data: dataArr
            // [
            //   { value: 25, name: '15%理财', itemStyle: { color: '#17acf0' }, labelLine: { normal: { show: false } } },
            //   { value: 20, name: '14%贷款', itemStyle: { color: '#ecd456' }, labelLine: { normal: { show: false } } },
            //   { value: 21, name: '8%网络婚恋，交友', itemStyle: { color: '#7d69ef' }, labelLine: { normal: { show: false } } },
            //   { value: 22, name: '21%刷单返利', itemStyle: { color: '#fab300' }, labelLine: { normal: { show: false } } },
            //   { value: 25, name: '40%赌博', itemStyle: { color: '#2dd3bd' }, labelLine: { normal: { show: false } } },
            // ]
          },
          {
            name: '外边框',
            type: 'pie',
            clockWise: false, //顺时加载
            hoverAnimation: false, //鼠标移入变大
            center: ['50%', '45%'],
            radius: ['80%', '80%'],
            label: {
              normal: {
                show: false
              }
            },
            data: [{
              value: 1,
              name: '',
              itemStyle: {
                normal: {
                  borderWidth: 1,
                  borderColor: '#0041bd'
                }
              }
            }],
          }
        ],

      }
      // this.loading1=false
      return option
    },

    drawbarChart(xdataArr,seriesDataArr,curMax,curInterval) {
      let bar_qx = this.$refs.mybarChart
      let barChart = this.$echarts.init(bar_qx)
      window.addEventListener('resize', function () {
        barChart.resize()
      })
      barChart.clear()
      barChart.setOption(this.setOptionBar(xdataArr,seriesDataArr,curMax,curInterval)) // 待完善：记得销毁echarts和resize
    },
    setOptionBar(xdataArr,seriesDataArr,curMax,curInterval) {
      let option = {
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'shadow'
          }
        },
        legend: {
          x: 'right',
          y: 'top',
          padding: [20, 10, 0, 0],
          orient: 'vertical',
          itemWidth: 20,
          itemHeight: 10,       
          textStyle:{
            color: '#12dae7',
            fontSize:11
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
          data: xdataArr,
          axisLine: {
            lineStyle: {
              color: "#384e5e",
            }
          },
          axisLabel:{
            textStyle:{
              color:'#7dc6d9'
            }
          },
          splitLine:{
            show: false
          },
          axisTick: {
            show: false // 隐藏 y 轴刻度线
          }
        },
        yAxis: {
          splitLine: {
            show: false
          },
          type: 'value',
          interval: curInterval,
          min: 0,
          max: curMax,
          axisLabel:{
            textStyle:{
              color:'#7dc6d9'
            }
          },
          axisLine: {
            lineStyle: {
              color: "#384e5e",
            }
          },
          axisTick: {
            show: false // 隐藏 y 轴刻度线
          }
        },
          
        
        series: seriesDataArr


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
    setOptionLine() {
      let option = {
        xAxis: {
          axisLabel: {
            textStyle:{
              color:'#fff'  // x轴字颜色 
            }
          },
          axisTick: {
            show: false // 隐藏 y 轴刻度线
          },
          axisLine: {
            lineStyle: {
              color: "#384e5e",  // x轴线颜色
            }
          },
         
          data: ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16', '17', '18', '19', '20', '21', '22', '23', '24', '25', '26', '27', '28', '29', '30']
        },
        grid: {
          top: '17%',
          left: '1%',
          right: '2%',
          bottom: '1%',
          containLabel: true
        },
        yAxis: [{
          type: 'value',
          // name: '星期/月',
          nameTextStyle: {
            color: '#fff' // 设置 Y 轴单位字的颜色
          },
          interval: 500,
          min: 0,
          max: 3500,
          axisLine: {
            lineStyle: {
              color: '#384e5e'
            }
          },
          axisLabel: {
            textStyle:{
              color:'#7d69ef'  // y轴字颜色 
            }
          },
          axisTick: {
            show: false // 隐藏 y 轴刻度线
          },
          splitLine: {
            lineStyle:{
              color:'#384e5e'
            }
          }
        },
        {
            type: 'value',
            interval: 100,
            min: 0,
            max: 700,
            axisLabel: {
              textStyle:{
                color:'#d3cb16'  // y轴字颜色 
              }
            },
            splitLine: {
              lineStyle:{
                color:'#384e5e'
              }
            }
          }
      ],
        legend:[{
          x: 'right',
          y: 'top',
          padding: [2, 300, 200, 100],
          orient: 'horizontal',
          textStyle:{
            color: function() {},
          }
        },
      ],
        series: [
          {
            name: '同源处理量',
            data: [1000, 2423, 2357, 2315, 1257, 2351, 3123, 1953, 2584, 1954, 2334, 2751, 2407, 3256, 1169, 2536, 2451, 2345, 2238, 3128, 3321, 1260, 2475, 2453, 1112, 2765, 1238, 2431, 1230, 2351],
            type: 'line',
            areaStyle: {},
            symbol: 'square',
            symbolSize: 5,
            itemStyle: {
              borderColor: '#4747a1',
              normal:{
                color: '#4747a1',
                lineStyle:
                {
                  color: '#4747a1'
                }
              }
            }
          },

          {
            name: '案件量',
            data: [1400, 2523, 2257, 2515, 3257, 1351, 3143, 1653, 2484, 1254, 2634, 2751, 2607, 3251, 1269, 3136, 1451, 3345, 1238, 3128, 1321, 2260, 3475, 1453, 3112, 1765, 3238, 1431, 3230, 1351],
            type: 'line',
            areaStyle: {},
            symbol: 'square',
            symbolSize: 5,
            itemStyle:
            {
              borderColor: '#7e861d',
              normal:
              {
                color: '#7e861d',
                lineStyle:
                {
                  color: '#7e861d'
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
  watch:{
    'mode':function(newVal){
      if(newVal == 'month'){
        this.dateRange=[
          dayjs().subtract(1, 'month').format('YYYY-MM-DD'),
          dayjs().format('YYYY-MM-DD')
        ]
      }else if(newVal == 'week'){
        this.dateRange=[
          dayjs().subtract(1, 'week').format('YYYY-MM-DD'),
          dayjs().format('YYYY-MM-DD')
        ]
      }
    },
    'dateRange':{
      handler:function(newVal){
        if(newVal[0] == dayjs().subtract(1, 'month').format('YYYY-MM-DD')){
          this.mode = 'month'
        }else if(newVal[0] == dayjs().subtract(1, 'week').format('YYYY-MM-DD')){
          this.mode = 'week'
        }else{
          this.mode = ''
        }
      },
      deep:true,
      immediate:true
    },
    // 'selectAll': {
    //   handler:function(newVal){
    //     if (newVal) {
    //       this.selectedItems = [...this.items];
    //     } else {
    //       this.selectedItems = [];
    //     }
    //   }
    // },
    'selectedItems': {
      handler(value) {
        if (value.length === this.items.length&& !value.includes(false)) {
          this.selectAll = true;
        } else {
          this.selectAll = false;
        }
        console.log(value);
      },
      deep: true
    },
  },
  
}



</script>



<style  scoped lang='less'>
@import '../common/font.css';

.transparent-button {
  cursor: pointer;
  background-color: transparent;
  border: none;
  color: white; /* 未选中状态为白色 */
}

.transparent-button.selected {
  color: #72e7ee; /* 选中状态为红色 */
}
  .el-date-editor .el-range-input {
  background-color: #052666;
  color: #c0c4cc;
}

/deep/.el-date-editor .el-range-separator{
  color:#0070cc
  }
.el-date-editor .el-range-input {
  color: #fdf6ec;
}
.el-date-editor .el-range-input,
.el-date-editor .el-range-separator {
  background-color: transparent;
  color: #c0c4cc;
}
.el-date-editor .el-range-input {
  color: #fdf6ec;
}
.el-input-width {
  width: 120px;
}
.el-input__inner {
  background-color: #0c2537;
  border: 1px solid #22739b;
  color: #b6e5ff;
}
.el-range-editor.is-active,
.el-range-editor.is-active:hover,
  .el-input__inner:focus,
  .el-select .el-input__inner:focus,
  .el-select .el-input.is-focus .el-input__inner {
  border-color: #22739b;
}
/deep/.el-date-editor .el-range-input,
.el-date-editor .el-range-separator {
  background-color: transparent;
  color: #c0c4cc;
}
/deep/.el-date-editor .el-range-input {
  color: #fdf6ec;
}
// 按钮hover
  /deep/ .el-button-chaxun:focus,
  /deep/ .el-button-chaxun:hover {
  background: url(../assets/img/shouye/查询按钮.png) no-repeat;
  background-size: 100% 100%;
}
  /deep/ .el-button-chongzhi:focus,
  /deep/ .el-button-chongzhi:hover {
  background: url(../assets/img/shouye/重置按钮.png) no-repeat;
  background-size: 100% 100%;
}
  /deep/ .el-button-daochu:focus,
  /deep/ .el-button-daochu:hover {
  background: url(../assets/img/shouye/下载按钮.png) no-repeat;
  background-size: 100% 100%;
}

.el-date-editor .el-range-input,
.el-date-editor .el-range-separator {
  background-color: #052666;
  color: #c0c4cc;
}
.el-date-editor .el-range-input,
.el-date-editor .el-range-separator {
  background-color: transparent;
  color: #c0c4cc;
}
.el-date-editor .el-range-input {
  color: #fdf6ec;
}
.el-date-editor .el-range-input {
  color: #fdf6ec;
}
.add1 {
  height: 300px;
  width: 600px;
}

.dp {
  width: 100%;
  height: 100%;
  background-color: #020723;
}

.top1 {
  width: 100%;
  height: 9.7%;
  position: relative;
}

.title {
  text-align: center;
  font-family: 'heiti';
  height: 75px;
  line-height: 75px;
  font-size: 42px;
  letter-spacing: 7px
    /* 12/16 */
  ;
  color: #19c9d5;
}

.top1_title {
  height: 100px;
  width: 100%;
  background-image: url('../assets/img/newbg/header_bg.png');
  background-repeat: no-repeat;
  // background-size: cover;  // 不会让图片走形，尽可能完整
  background-size: 100% 100%; // 根据容器大小拉伸图片，会让图片走形
}

.center2 {
  display: flex;
  justify-content: space-between;
  margin: 0 36px;
}

.gsh3 {
  margin-top: 28px;
  margin-left: 28px;
  font-size: 20px;
}

.center2_left,
.center2_center,
.center2_right {
  font-size: 20px;
  color: #32c5ff;
  height: 360px; // width/height=1.58
  width: 600px;
  margin-top: 20px;
  margin-left: 20px;
  margin-right: 30px;
  background: url(../assets/img/newbg/kuangbg.png);
  background-size: 100% 100%;
  background-repeat: no-repeat;
}

.center2_left_test1 {
  color: #72e7ee;
  margin-left: 38px;
}

.center2_left_test2 {
  color: #c9c391;
  margin-top: 14px;
  margin-left: 38px;
}

.picture_test1 {
  // display: inline-block;
  background-image: url(../assets/img/newbg/lanbg2.png);
  background-size: 100% 100%;
  width: 52px;
  height: 52px;
  line-height: 52px;
  text-align: center;
  margin-left: 68px;
  margin-top: 20px;
}

.picture_test2 {
  // display: inline-block;
  background-image: url(../assets/img/newbg/lanbg2.png);
  background-size: 100% 100%;
  width: 52px;
  height: 52px;
  line-height: 52px;
  text-align: center;
  margin-left: 13px;
  margin-top: 20px;
}

.picture_test3 {
  display: inline-block;
  background-image: url(../assets/img/newbg/weixin2.png);
  background-size: 100% 100%;
  width: 52px;
  height: 52px;
  line-height: 52px;
  text-align: center;
  margin-left: 68px;
  margin-top: 20px;
}

.picture_test4 {
  display: inline-block;
  background-image: url(../assets/img/newbg/weixin2.png);
  background-size: 100% 100%;
  width: 52px;
  height: 52px;
  line-height: 52px;
  text-align: center;
  margin-left: 13px;
  margin-top: 20px;
}

.center2_left_test3 {
  display: inline-block;
  // margin-left: 30px;
  color: #18c5e7;
  // vertical-align:super;
  transform: translate(360px, -40px);
  font-size: 20px;
}

.picture_test12 {
  vertical-align: text-top;
  width: 14px;
  transform: translate(370px, -35px);
}

.pos {
  display: inline-block;
  margin-left: 90px;
  font-size: 19px;
  color: #fff;
  
}

.pos1 {
  color: #72e7ee;
  margin-top: 7px;
}

.box {
  border-style: solid;
  width: 21px;
  height: 11px;
  border: 2px solid blue;
  outline: none;
}

.center3 {
  margin-top: 30px;
  margin-left: 55px;
  margin-right: 65px
}

.center3_one {
  font-size: 17px;
  height: 356px;
  width: 100%;
  background: url(../assets/img/newbg/kuangbg2.png);
  background-size: 100% 100%;
  background-repeat: no-repeat;
  color: #32c5ff;
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

  // background-color: pink;
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

.classA {
  color: rgba(0, 255, 243, 0.75);
  font-size: 20px
}

.classA:hover {
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
  color: #00fff3 // box-shadow: 0 0 10px #1768be inset, 0 0 0px #b5c5d4;
}

.nav_li .daohang3 li span {
  color: #00fff3
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
  position: fixed;
  bottom: 8px;
  height: 6vh;
  width: 100%;
  background: url(../assets/img/newbg/bottombg.png);
  background-size: 100% 100%;
  background-repeat: no-repeat;
}

.fanhui {
  position: absolute;
  left: 3.5%;
  top: 15%;
  color: #11d8e5;
  font-size: 20px;
}

.fanhui img {
  width: 75px;
  height: 40px;
  // background-color: rgb(209, 77, 94);
}

.topguanli {
  width: 8.1%
    /* 150/16 */
    /* 277/16 */
  ;
  height: 50%
    /* 30/16 */
    /* 35/16 */
  ;
  position: absolute;
  right: 0%;
  top: 20%;
}

.role {
  height: 30px
    /* 30/16 */
  ;
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