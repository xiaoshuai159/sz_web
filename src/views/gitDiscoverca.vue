<template>
  <!-- 长安发现 -->
  <!-- 长安发现 -->
  <!-- 长安发现 -->
  <div class="right_main_under">
    <Navlist style="transform: translate( 0,-0.08rem);"> </Navlist>

    <div class="search_select_form bg">
      <template>
        <el-form
          :inline="true"
          :model="formInline"
          class="demo-form-inline"
          size="mini"
        >
          <!-- 发现日期 -->
          <el-form-item label="发现日期">
            <el-date-picker
              v-model="newdomainSimpleVo.dateRange"
              type="daterange"
              :clearable="false"
              range-separator="至"
              start-placeholder="开始日期"
              end-placeholder="结束日期"
              value-format="yyyy-MM-dd"
            >
            </el-date-picker>
          </el-form-item>


          <!-- 诈骗大类 -->
          <el-form-item label="诈骗类型">
            <el-select v-model.trim="newdomainSimpleVo.fraudType" clearable placeholder="诈骗类型">
              <el-option

                v-for="item in fraudTypeOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </el-form-item>


          <el-form-item label="目的IP">
            <el-input v-model.trim="newdomainSimpleVo.destIP" placeholder="IP">
            </el-input>
          </el-form-item>

          <el-form-item label="域名">
            <el-input v-model.trim="newdomainSimpleVo.url" placeholder="域名">
            </el-input>
          </el-form-item>

          <el-form-item>
            <el-button
              type="primary"
              class="el-button-daochu"
              size="mini"
              @click.native.stop="searchTabData"
              >查询</el-button
            >
            <el-button
              class="el-button-daochu"
              type="primary"
              size="mini"
              @click.native="resetFun"
              >重置</el-button
            >
            <el-button
              class="el-button-daochu"
              type="primary"
              size="mini"
              @click.native="put"
              :loading="loadingbut"              
              >{{ loadingbuttext }}</el-button
            >
            <!-- v-if="getRole1('downloadRaw')" :disabled="this.tableData.length == 0"  7.4 测试 -->
            <!-- </template> -->
          </el-form-item>
        </el-form>
      </template>
    </div>
    <!-- //列表 -->
    <Err v-if="errFlag"></Err>
    <!-- <div class="list_xia"> -->
    <el-table
      :row-class-name="tableRowClassName"
      :row-key="getRowKeys"
      ref="multipleTable"
      :data="tableData"
      style="width: 100%"
      :height="heights"
      id="onetable"
      size="mini"
      class="tableStyle"
      @selection-change="handleSelectionChange"
    >
      <el-table-column label="序号" type="index" width="70">
        <template slot-scope="scope">
          <span v-if="scope.row.discoverDate">
            {{ scope.$index + 1 }}
          </span>
        </template>
      </el-table-column>
      <!-- :reserve-selection="true" -->
      <el-table-column label="发现日期" prop="discoverDate" min-width="90" show-overflow-tooltip>
        <!-- min-width="10%" -->
      </el-table-column>
      <el-table-column label="诈骗类型" prop="fraudType" min-width="170" show-overflow-tooltip>
      </el-table-column>
      <el-table-column label="域名" prop="url" width="200" show-overflow-tooltip> </el-table-column>
      <el-table-column label="目的IP" prop="dstIp" min-width="120" show-overflow-tooltip> </el-table-column>
      <el-table-column label="访问量" prop="visits" min-width="80" show-overflow-tooltip> </el-table-column>
    </el-table>


    
    <!-- //分页 -->
    <div class="bottom">
      <div class="ss">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="mypageable.pageNum"
          :page-sizes="[15, 30, 45]"
          :page-size="mypageable.pageSize"
          layout="total, prev, pager, next, jumper"
          :total="total"
          class="pagePagination pageRight"
        >
        </el-pagination>
      </div>
    </div>
    <!-- </div> -->

    <!-- 截图 -->

    <el-dialog
      :title="jietutitle"
      :visible.sync="newkanjietu"
      width="45%"
      height="40%"
      :before-close="newkanjietuclose"
      :close-on-click-modal="false"
    >
      <img
        v-if="!this.xinshi"
        :src="this.jieURL"
        ref="img"
        alt=""
        class="img"
      />
      <div v-if="this.xinshi" class="xianshititle">{{ xianshititle }}</div>
    </el-dialog>
  </div>
</template>





<script>
import Navlist from '@/components/hearderdongtainav.vue'
import getRole from '@/utils/promission.js'
import Err from '@/components/err.vue'
import dayjs from 'dayjs'
export default {
  // inject: ["reload"],
  name: 'search',
  components: {},
  data() {
    return {
      tableData:[],
      newdomainSimpleVo:{
        dateRange:[ dayjs().subtract(1, 'month').format('YYYY-MM-DD'), dayjs().format('YYYY-MM-DD')],
        fraudType:null,
        url:null,
        destIP:null,
        // isValidIP: true

      },
     
      fraudTypeOptions:[],
      heights: undefined,
      errFlag: false,
      xianshi: false,
      errFlagTimer: undefined,
      loadingbuttext: '导出',
      loadingbut: false,
      loading: true,
      gridData: [
        // {
        //   sourceIp:12009
        // }
      ],
      whiteSearchList: {
        startCreateTime:
          dayjs().subtract(1, 'month').format('YYYY-MM-DD') ,
        endCreateTime:
          dayjs().format('YYYY-MM-DD')
      },
      isLoading: false,

      dialogFormVisible: false,

      formInline: {
        user: '',
        region: '',
      },


      domainFeedbackVo: {
        accessSystemType: null,
        feedbackStatus: null,
      },

      mypageable: {
        pageNum: 1,
        pageSize: 10,
      },
      mypageable1: {
        pageNum1: 1,
        pageSize1: 10,
      },
      total1: 1,
      total: 0,
      totalPages: '',
      totalPages1: '',
      newkanjietu: false,
      jietutitle: '',
      jieURL: '',
      xianshititle: '暂无图片',
      clicktitle: '点击查看图片',
      xinshi: false,

        // 改之后：单级别下拉框
        options:[
          {
              value: 'DK',
              label: '贷款',
            },
            {
              value: 'SD',
              label: '刷单返利类',
            },
            {
              value: 'GJF',
              label: '冒充公检法类',
            },
            {
              value: 'LC',
              label: '理财',
            },
            {
              value: 'SZP',
              label: '杀猪盘',
            },
            {
              value: 'KF',
              label: '冒充客服类',
            },
            {
              value: 'DS',
              label: '电商类诈骗',
            },
            {
              value: 'JY',
              label: '网络婚恋、交友类',
            },
            {
              value: 'ZX',
              label: '虚假征信类',
            },
            {
              value: 'YX',
              label: '网络游戏产品虚假交易类',
            },
        ],
      selectData: {
        sourceTypeData: [
          // { value: 'CA', label: '长安' },
          // { value: 'YC', label: '4G' },
          { value: 'ZW', label: '中卫' },
        ],

        fraudypeData: [
          { value: 'DK', label: '贷款代办信用卡类' },
          { value: 'SD', label: '刷单返利类' },
          { value: 'GJF', label: '冒充公检法类' },

          // { value: "GW", label: "网络购物" },
          { value: 'OTHER', label: '其他类型诈骗' },
          { value: 'KF', label: '冒充客服类' },
          { value: 'JJGW', label: '冒充军警购物诈骗' },
          { value: 'SZP', label: '杀猪盘' },
          { value: 'DS', label: '电商类诈骗' },
          { value: 'JY', label: '网络婚恋、交友类' },
          { value: 'ZX', label: '虚假征信类' },
          { value: 'MC', label: '冒充领导、熟人类' },
          { value: 'YX', label: '网络游戏产品虚假交易类' },
          { value: 'APP', label: '分发平台' },
          { value: 'XZYM', label: '下载页面' },
        ],
      },
      
      tableDatalist: [],

      newurl: '',
      arr: [],
      yuming: '',
    }
  },
  components: {
    Navlist: Navlist, //将别名demo 变成 组件 Demo
    Err: Err,
  },

  computed: {},
  mounted() {
    this.yangshi()
    this.getTabData()
    // setInterval(function () {
    // document
    //   .querySelectorAll('.el-cascader-panel .el-radio ')
    //   .forEach((val) => {
    //     val.style.display = 'none'
    //   })
    // }, 10)

    // console.log();
    //层级下拉框
    // setInterval(function () {
    //   document.querySelectorAll('.el-cascader-node__label').forEach((el) => {
    //     el.onclick = function () {
    //       if (this.previousElementSibling) this.previousElementSibling.click()
    //     }
    //   })
    // }, 1000)
  },
  created(){
    this.getOptionsData()
  },
  methods: {
    


    async getOptionsData(){
      const {data:res} = await this.$http.get('/dict/fraudType')
      if(res.code == 200){
        const fraudTypeArr = res.data
        fraudTypeArr!=null && fraudTypeArr.forEach(fraudType=>this.fraudTypeOptions.push({key:fraudType, value:fraudType}))
      }else{
        this.$message(res.data)
      }
    },
    yangshi() {
      this.heights =
        window.innerHeight - this.$refs.multipleTable.$el.offsetTop - 270
      document.querySelector('#onetable .el-table__body tbody').style.height =
        ((this.heights - 5) / 10) * 9 + 'px' // var rows = document.querySelectorAll('#onetable .el-table__row ') // // console.log( document.querySelectorAll('#onetable .el-table__row ')); // rows.forEach((item) => { //   item.style.height = Math.floor(this.heights / 11) + 'px' // })

      document.querySelector('#onetable .has-gutter tr ').style.height =
        Math.floor(this.heights / 11) + 'px' // // 监听浏览器高度变化，修改表格高度

      window.onresize = () => {
        this.heights =
          window.innerHeight - this.$refs.multipleTable.$el.offsetTop - 270

        document.querySelector('#onetable .el-table__body tbody').style.height =
          ((this.heights - 5) / 10) * 9 + 'px' // var rows = document.querySelectorAll('#onetable .el-table__row ') // rows.for
      }
    },
    //查看截图
    jietu(time, userimg) {
      this.jietutitle = userimg
      this.imgtu(time, userimg)
      this.newkanjietu = true
    },
    //获取截图
    async imgtu(time, userimg) {
      const { data: res } = await this.$http.get('/discover/getImage', {
        params: {
          imageName: userimg,
          disDate: dayjs(time).format('YYYY/MM/DD'),
        },
      })
      if (res.code == 200) {
        if (res.data != null) {
          this.xinshi = false
          this.jieURL = res.data
        } else {
          this.xinshi = true
        }
      }
    },
    newkanjietuclose() {
      this.newkanjietu = false
    },
    //层级下拉框
    handleChange(val) {
      //  //地区选择之后将下拉框界面收起
      if (this.newdomainSimpleVo.type != null) {
        this.$refs.cascader.toggleDropDownVisible()
        if (this.newdomainSimpleVo.type.length > 1) {
          this.newdomainSimpleVo.typesmall = this.newdomainSimpleVo.type[1]
          this.newdomainSimpleVo.typebig = null
        } else {
          this.newdomainSimpleVo.typebig = this.newdomainSimpleVo.type[0]
          this.newdomainSimpleVo.typesmall = null
        }
      }
    },
    getRole1(data) {
      return getRole(data)
      // console.log( getRole(data));
    },
    //初始化获取数据
    async getTabData() {
      // get 请求第一个参数是接口地址'/discover/list'，第二个参数是 请求体{ params：data }
      // post 请求第一个参数是接口地址'/discover/list'，第二个参数是 请求体 data
      const { data: res } = await this.$http.get('/discover/list', {
        params:{
          discoverDateStart:this.newdomainSimpleVo.dateRange[0],
          discoverDateEnd:this.newdomainSimpleVo.dateRange[1],
          fraudType:this.newdomainSimpleVo.fraudType,
          dstIp:this.newdomainSimpleVo.destIP,
          url:this.newdomainSimpleVo.url,
          page:this.mypageable.pageNum,
          pageSize:this.mypageable.pageSize
        }
      })
      if(res.code == 200){
        this.tableData = res.dataList
        let tableDataLength = this.tableData.length
        let timer = null
        timer ? clearTimeout(timer) : ''
        if (this.tableData.length < 10) {
          for (var i = this.tableData.length; i < 10; i++) {
            this.tableData.push({})
          }
        }
        if (tableDataLength < 10) {
          timer = setTimeout(() => {
            for (var i = tableDataLength; i < 10; i++) {
              document.querySelectorAll('#onetable tbody .el-checkbox')[
                i
              ].style.display = 'none'
            }
          })
        }
        this.total = res.totalSum
        this.totalPages = res.totalPage // }else{ //   this.$message('无数据') // }
      
      }
      // 请求体内容
      // let getlist = {
      //   discoverDateStart:this.newdomainSimpleVo.dateRange[0],
      //   discoverDateEnd:this.newdomainSimpleVo.dateRange[1],
      //   fraudType:this.newdomainSimpleVo.fraudType,
      //   page,
      //   pageSize
      // }
      // console.log(getlist);
      // const { data: res } = await this.$http.get(
      //   '/discover/list',
      //   {params:getlist}
      // )
      // if (res.code == 200) {
      //   this.tableData = res.dataList
      //   let tableDataLength = this.tableData.length
      //   let timer = null
      //   timer ? clearTimeout(timer) : ''
      //   if (this.tableData.length < 10) {
      //     for (var i = this.tableData.length; i < 10; i++) {
      //       this.tableData.push({})
      //     }
      //   }
      //   if (tableDataLength < 10) {
      //     timer = setTimeout(() => {
      //       for (var i = tableDataLength; i < 10; i++) {
      //         document.querySelectorAll('#onetable tbody .el-checkbox')[
      //           i
      //         ].style.display = 'none'
      //       }
      //     })
      //   }
      //   this.total = res.data.totalElements
      //   this.totalPages = res.data.totalPages // }else{ //   this.$message('无数据') // }
      // }
    },
    // 自定义函数判断IP地址是否正确
    isIPAddress(ip) 
    {
      let regExp = /^((25[0-5]|2[0-4]\d|1\d{2}|[1-9]?\d)\.){3}(25[0-5]|2[0-4]\d|1\d{2}|[1-9]?\d)$/;
      return regExp.test(ip);
    },
    //查询
    async searchTabData() {
      if(!this.newdomainSimpleVo.destIP || this.isIPAddress(this.newdomainSimpleVo.destIP)){
        this.mypageable.pageNum = 1
        this.getTabData()
      }else{
        this.$message('ip格式输入错误')
      }

      // this.getTabData()
      // this.resetFun()
      // let getlist = {
      //   startDiscoverDate: this.whiteSearchList.startCreateTime,
      //   endDiscoverDate: this.whiteSearchList.endCreateTime,
      //   mypageable: this.mypageable,
      //   // url: this.newdomainSimpleVo.url,
      //   type: this.newdomainSimpleVo.url,
      //   visits: this.newdomainSimpleVo.visits,
      // }
      // const { data: res } = await this.$http.post(
      //   '/discover/getDiscover',
      //   getlist
      // )
      // // console.log(res);
      // if (res.code == 200) {
      //   this.mypageable.pageNum = 1
      //   this.tableData = res.data.content
      //   this.total = res.data.totalElements
      //   this.totalPages = res.data.totalPages
      // } else {
      //   this.$message('无数据')
      //   this.mypageable.pageNum = 1
      //   this.mypageable.pageSize = 10
      //   this.getTabData()
      //   this.resetFun()
      // }
    },

    //重置方法
    resetFun() {
      this.newdomainSimpleVo.dateRange=[ dayjs().subtract(1, 'month').format('YYYY-MM-DD'), dayjs().format('YYYY-MM-DD')],
      this.newdomainSimpleVo.fraudType=null,
      this.newdomainSimpleVo.destIP=null,
      this.newdomainSimpleVo.url=null,
      this.mypageable.pageNum = 1
      this.mypageable.pageSize = 10
      this.getTabData()
    },
    handleSelectionChange(val) {
      this.tableDatalist = val
    },

    // newput() {
    //   if (this.tableDatalist.length == 0) {
    //     this.$confirm(
    //       '您可以对数据进行勾选下载，若要下载全部发现数据点击确定?',
    //       '提示',
    //       {
    //         confirmButtonText: '确定',
    //         cancelButtonText: '取消',
    //         type: 'warning',
    //       }
    //     )
    //       .then(async () => {
    //         this.put()
    //       })
    //       .catch(() => {
    //         this.$message('已取消')
    //       })
    //   } else {
    //     this.put()
    //   }
    // },
    //下载   //文件流
    async put() {
      let getlist = {

        discoverDateStart :this.newdomainSimpleVo.dateRange[0],
        discoverDateEnd:this.newdomainSimpleVo.dateRange[1],
        fraudType:this.newdomainSimpleVo.fraudType,
        dstIp:this.newdomainSimpleVo.destIP,
        url:this.newdomainSimpleVo.url,
        // discoverTimeDTO: {
        //   startTime: this.whiteSearchList.startCreateTime,
        //   endTime: this.whiteSearchList.endCreateTime,
        // },

        // pageable: this.mypageable,
        // // sourceEnum: this.newdomainSimpleVo.source,
        // type: this.newdomainSimpleVo.type,
        // url: this.newdomainSimpleVo.url,
        // // category: this.newdomainSimpleVo.type,
      }
      this.loadingbuttext = '...加载中'
      this.loadingbut = true
      this.$http({
        method: 'GET',
        url: '/discover/export',
        responseType: 'blob',
        params: getlist,
      })
        .then((res) => {
          let that = this
          let blob = res.data
          if (blob.type == 'application/json') {
            const reader = new FileReader()
            reader.onload = function () {
              that.$message('下载文件失败')
              that.loadingbuttext = '导出'
              that.loadingbut = false
            }
            reader.readAsText(blob)
          } else {
            const s_date = dayjs(this.newdomainSimpleVo.dateRange[0]).format('YYYYMMDD'), f_date = dayjs(this.newdomainSimpleVo.dateRange[1]).format('YYYYMMDD'), fraudType = this.newdomainSimpleVo.fraudType == null ? '' : this.newdomainSimpleVo.fraudType, destIP = this.newdomainSimpleVo.destIP== null ? '' : this.newdomainSimpleVo.destIP, u = this.newdomainSimpleVo.url== null ? '' : this.newdomainSimpleVo.url;
            let title = s_date + '至' + f_date
            if (fraudType !== '') {
              title += '-' + fraudType;
            }

            if (destIP !== '') {
              title += '-' + destIP;
            }

            if (u !== '') {
              title += '-' + u;
            }

            title += '-发现导出.xlsx';
            // let title = dayjs().format('YYYYMMDD') + '发现导出.xlsx'
            let binaryData = []
            binaryData.push(blob)
            let url = window.URL.createObjectURL(new Blob(binaryData), {
              type: 'application/vnd.ms-excel',
            })
            const aLink = document.createElement('a')
            aLink.href = url
            aLink.setAttribute('download', title)
            document.body.appendChild(aLink)
            aLink.click()
            this.loadingbuttext = '导出'
            this.loadingbut = false
            this.$message.success('文件导出成功！')
            document.body.removeChild(aLink)
          }
        })
        .catch((err) => {
          console.log(err);
          this.$message.error('文件下载失败！')
          this.loadingbuttext = '导出'
          this.loadingbut = false
        })
    },
    //访问量
    // async fangwenl(val) {
    //   this.gridData = []
    //   this.loading = true
    //   this.yuming = val.url
    //   this.arr.push(val.id)
    //   this.dialogTableVisible = true
    //   let list = {
    //     mypageable: {
    //       pageNum: this.mypageable1.pageNum1,
    //       pageSize: this.mypageable1.pageSize1,
    //     },
    //     masterIds: this.arr,
    //     endDiscoverTime: null,
    //     startDiscoverTime: null,
    //   }
    //   const { data: res } = await this.$http.post('/discover/getRawData', list)
    //   if (res.code == 200) {
    //     this.loading = false
    //     this.gridData = res.data.content
    //     this.total1 = res.data.totalElements
    //     this.totalPages1 = res.data.totalPages
    //   }
    // },
    handleSizeChange(val) {
      // console.log(val);
      this.mypageable.pageSize = val
      this.getTabData()
    },
    // async   handleSizeChange1(val) {
    //   // console.log(val);
    //   this.mypageable1.pageSize1 = val;
    //   // this.fangwenl();
    //    let list={
    //  mypageable:{
    //    pageNum:this.mypageable1.pageNum1,
    //    pageSize:this.mypageable1.pageSize1
    //  },

    //  masterIds:this.arr,
    //  endDiscoverTime: null,
    //  startDiscoverTime: null,
    //   }
    //   const {data:res}= await this.$http.post('/discover/getRawData',list)
    //   if(res.code==200){
    //     this.gridData=res.data.content
    //       this.total1 = res.data.totalElements;
    //     this.totalPages1 = res.data.totalPages;
    //   }
    // },
    handleCurrentChange(val) {
      // console.log(val, 111);

      this.mypageable.pageNum = val

      // console.log( this.mypageable.pageNum);
      this.getTabData()
    },
    async handleCurrentChange1(val) {
      this.gridData = []
      this.loading = true
      this.mypageable1.pageNum1 = val

      // console.log( this.mypageable.pageNum);
      // this.fangwenl();
      let list = {
        mypageable: {
          pageNum: this.mypageable1.pageNum1,
          pageSize: this.mypageable1.pageSize1,
        },
        masterIds: this.arr,
        endDiscoverTime: null,
        startDiscoverTime: null,
      }
      const { data: res } = await this.$http.post('/discover/getRawData', list)
      if (res.code == 200) {
        this.loading = false
        this.gridData = res.data.content
        this.total1 = res.data.totalElements
        this.totalPages1 = res.data.totalPages
      }
    },
    modelType1_clearFun(val) {
      if (val == '') {
        this.newdomainSimpleVo.source = null
      }
    },

    dataCreate_change(val) {
      if (val && val != '') {
        this.whiteSearchList.startCreateTime = val[0]
        this.whiteSearchList.endCreateTime = val[1]
      } else {
        this.whiteSearchList.startCreateTime = null
        this.whiteSearchList.endCreateTime = null
      }
    },
    // handleClose(done) {
    //   this.mypageable1.pageNum1 = 1
    //   this.arr = []
    //   this.dialogTableVisible = false
    // },
    //    eldialogout() {
    //   (this.isShow = false), this.$refs.multipleTable.clearSelection(); //清除选中的数据
    // },
    getRowKeys(row) {
      return row.id
    },
    bigtype(val) {
      var status = ''
      switch (val) {
        case 'KF':
          status = '冒充客服类'
          break
        case 'GJF':
          status = '冒充公检法类'
          break
        case 'SD':
          status = '刷单返利类'
          break
        case 'DK':
          status = '贷款代办信用卡类'
          break
        case 'JJGW':
          status = '冒充军警购物诈骗'
          break
        case 'SZP':
          status = '杀猪盘'
          break
        case 'DS':
          status = '电商类诈骗'
          break
        case 'LC':
          status = '理财'
          break
        case 'JY':
          status = '网络婚恋、交友类'
          break
        case 'ZX':
          status = '虚假征信类'
          break
        case 'MC':
          status = '冒充领导、熟人类'
          break
        case 'YX':
          status = '网络游戏产品虚假交易类'
          break
        case 'OTHER':
          status = '其他类型诈骗'
          break
        case 'APP':
          status = '分发平台'
          break
        case 'XZYM':
          status = '下载页面'
          break
        case 'HC':
          status = '灰产'
          break
        default:
          status = val
          break
      }
      return status
    },

    smalltype(val) {
      var status = ''
      switch (val) {
        case 'kf_ds':
          status = '冒充电商客服'
          break
        case 'kf_wl':
          status = '冒充物流客服'
          break
        case 'kf_other':
          status = '冒充其他客服类'
          break
        case 'gjf_mc':
          status = '冒充公检法'
          break
        case 'gjf_gs':
          status = '工商平台类'
          break
        case 'gjf_etc':
          status = 'ETC通行卡'
          break
        case 'gjf_other':
          status = '其他政府机关或单位组织'
          break
        case 'sd_cz':
          status = '充值（红包）返利'
          break

        case 'dk_xyk':
          status = '虚假代办信用卡'
          break
        case 'dk_te':
          status = '虚假提额套现'
          break
        case 'dk_dk':
          status = '虚假贷款'
          break
        case 'dk_other':
          status = '其他贷款类'
          break
        case 'szp_lc':
          status = '虚假投资理财类'
          break
        case 'szp_dubo':
          status = '博彩彩票'
          break
        case 'szp_ty':
          status = '体育直播，比分竞猜'
          break
        case 'szp_yx':
          status = '棋牌游戏'
          break
        case 'ds_gw':
          status = '虚假购物'
          break
        case 'ds_fw':
          status = '虚假服务'
          break
        case 'ds_other':
          status = '其他电商类'
          break
        case 'jy_jr':
          status = '冒充外国军人'
          break
        case 'jy_hl':
          status = '网络婚恋'
          break
        case 'jy_jy':
          status = '网络交友，聊天交友'
          break
        case 'jy_other':
          status = '其他交友类'
          break
        case 'zx_xyd':
          status = '消除校园贷记录'
          break
        case 'zx_bljl':
          status = '消除不良记录'
          break
        case 'zx_other':
          status = '其他征信'
          break
        case 'mc_ld':
          status = '冒充领导'
          break
        case 'mc_sr':
          status = '冒充熟人'
          break
        case 'mc_gz':
          status = '冒充公众人物'
          break
        case 'mc_other':
          status = '冒充其他身份类'
          break
        case 'yx_card':
          status = '游戏币、游戏点卡诈骗'
          break
        case 'yx_zhzb':
          status = '游戏账号、游戏装备诈骗'
          break
        case 'yx_other':
          status = '其他游戏产品虚假交易'
          break
        case 'other_zj':
          status = '虚假中奖诈骗'
          break
        case 'other_zp':
          status = '虚假招聘'
          break
        case 'other_jp':
          status = '机票退改签诈骗'
          break
        case 'other_tp':
          status = 'ps图片诈骗'
          break
        case 'jjgw':
          status = '冒充军警购物诈骗'
          break
        case 'app_ff':
          status = '分发平台'
          break
        case 'xzym':
          status = '下载页面'
          break
        case 'hc_fw':
          status = '灰产服务'
          break
        case 'hc_other':
          status = '灰产其他'
          break
        case 'hc':
          status = '灰产'
          break
        case 'kf':
          status = '冒充客服类'
          break
        case 'gjf':
          status = '冒充公检法类'
          break
        case 'sd':
          status = '刷单返利类'
          break
        case 'dk':
          status = '贷款代办信用卡类'
          break
        case 'jjgw':
          status = '冒充军警购物诈骗'
          break
        case 'szp':
          status = '杀猪盘'
          break
        // case 'DS':
        //   status = '电商类诈骗'
        //   break
        case 'jy':
          status = '网络婚恋、交友类'
          break
        case 'zx':
          status = '虚假征信类'
          break
        case 'mc':
          status = '冒充领导、熟人类'
          break
        case 'yx':
          status = '网络游戏产品虚假交易类'
          break
        case 'other':
          status = '其他类型诈骗'
          break
        case 'app':
          status = '分发平台'
          break
        case 'xzym':
          status = '下载页面'
          break

        default:
          status = val
          break
      }
      return status
    },

    zP(val) {
      if (val == 'DK') {
        return '贷款代办信用卡类'
      } else if (val == 'SD') {
        return '刷单返利类'
      } else if (val == 'GJF') {
        return '冒充公检法类'
      } else if (val == 'KF') {
        return '冒充客服类'
      } else if (val == 'OTHER') {
        return '其他类型诈骗'
      } else if (val == 'JJGW') {
        return '冒充军警购物诈骗'
      } else if (val == 'SZP') {
        return '杀猪盘'
      } else if (val == 'DS') {
        return '电商类诈骗'
      } else if (val == 'JY') {
        return '网络婚恋、交友类'
      } else if (val == 'ZX') {
        return '虚假征信类'
      } else if (val == 'MC') {
        return '冒充领导、熟人类'
      } else if (val == 'YX') {
        return '网络游戏产品虚假交易类'
      } else if (val == 'APP') {
        return '分发平台'
      } else if (val == 'XZYM') {
        return '下载页面'
      } else {
        return val
      }
    },
    tableRowClassName({ rowIndex }) {
      if (rowIndex % 2 === 0) {
        return 'warning-row'
      } else if (rowIndex % 2 === 1) {
        return 'success-row'
      }
      return ''
    },
    // 转ip
    zhuanip(num) {
      var str
      var tt = new Array()
      tt[0] = (num >>> 24) >>> 0
      tt[1] = ((num << 8) >>> 24) >>> 0
      tt[2] = (num << 16) >>> 24
      tt[3] = (num << 24) >>> 24
      str =
        String(tt[0]) +
        '.' +
        String(tt[1]) +
        '.' +
        String(tt[2]) +
        '.' +
        String(tt[3])
      return str
    },
  },
}
</script>

<style  scoped lang='less'>
/deep/ .el-form-item__label {
  font-size:0.085rem
}
.right_main_under /deep/ .el-button-chaxun:focus,
.right_main_under /deep/ .el-button-chaxun:hover {
  background: url(../assets/img/shouye/查询按钮.png) no-repeat;
  background-size: 100% 100%;
}
.right_main_under /deep/ .el-button-chongzhi:focus,
.right_main_under /deep/ .el-button-chongzhi:hover {
  background: url(../assets/img/shouye/重置按钮.png) no-repeat;
  background-size: 100% 100%;
}
.right_main_under /deep/ .el-button-daochu:focus,
.right_main_under /deep/ .el-button-daochu:hover {
  background: url(../assets/img/shouye/下载按钮.png) no-repeat;
  background-size: 100% 100%;
}
/deep/ .el-table__fixed-right::before,
.el-table__fixed::before {
  background-color: #192d45;
}
/deep/.el-table--enable-row-hover .el-table__body tr:hover > td {
  background-color: transparent;
}

/deep/.el-table--border::after,
.el-table--group::after,
.el-table::before {
  background-color: #192d45 !important;
}
.el-pagination {
  text-align: right;
}
.bottom {
  width: 100%;
  height: 40px /* 60/16 */ /* 40/16 */;
  background-color: rgba(11, 48, 78, 0.6);
  padding-bottom: 1%;
  .ss_l {
    float: left;
    margin-top: 13px;
    span {
      margin-left: 1.25rem /* 10/16 */;
      margin-right: 1.25rem /* 20/16 */;
      a {
        color: #fff;
      }
      a:hover {
        color: red;
      }
    }
  }
  .ss {
    float: right;
  }
}

.bottom1 {
  width: 100%;
  height: 50px /* 60/16 */ /* 40/16 */;

  box-sizing: border-box;
  .ss1 {
    float: right;
    margin-right: 46px /* 46/16 */;
  }
}
.warning {
  display: flex;
  justify-content: space-around;
  width: 100%;
  height: 5rem /* 80/16 */;
  .item {
    .el-button {
      width: 9.375rem /* 150/16 */ /* 100/16 */;
      height: 4rem /* 80/16 */;
      font-size: 18px;
      background-color: rgb(93, 93, 199);
      color: #fff;
      border: 1px solid transparent;
    }
  }
}

// * el-divider 修改高度&虚线效果 */
.el-divider--horizontal {
  margin-top: 0.625rem /* 10/16 */;
  background: 0 0;
  // border-top: 1px solid #596168;
}
.dialist {
  padding: 0 1.25rem /* 10/16 */ 1.25rem 0 /* 20/16 */;
  .ss {
    margin-right: 1.5rem /* 20/16 */;
  }
}
// /deep/.el-dialog {
//   margin: 5vh auto !important;
// }

// /deep/ .el-dialog__body {
//   height: 70vh;
//   overflow: auto;
// }
.urlcolor {
  color: #909090;
}
.tubiao {
  width: 100% /* 1558/16 */;
  height: 12.5rem /* 200/16 */ /* 300/16 */;
}
.tubiao1 {
  width: 100% /* 1558/16 */;
  height: 10rem /* 200/16 */ /* 300/16 */;
  display: flex;
  justify-content: space-around;
}
.left {
  width: 40% /* 779/16 */;
  height: 10rem /* 300/16 */;
  // float: left;
  // padding: 1.25rem /* 20/16 */ /* 20/16 */ /* 50/16 */ 3.125rem /* 50/16 */
  /* 100/16 */ /* 40/16 */
  box-sizing: border-box;
  background-color: #07293f;
}
#bar_qx {
  height: 10rem /* 240/16 */ /* 260/16 */ /* 200/16 */;
  padding-top: 0.625rem /* 10/16 */;
  margin-left: 0.625rem /* 10/16 */ /* 20/16 */;
}
#bar_zz {
  height: 10rem /* 200/16 */;
  // margin-left: 1.875rem /* 30/16 */ /* 20/16 */;
  margin-left: 0.625rem;
}
.right {
  width: 40%;
  height: 10rem;

  // float: right;
  // padding: 1.25rem /* 20/16 */ /* 50/16 */ 3.125rem /* 100/16 */;
  box-sizing: border-box;
  background-color: #07293f;
}
.el-table__body /deep/ .el-button--mini {
  color: #fff;
}
// .el-table::before {
//   height: 10px;
// }
// .dialogInfo /deep/ .el-table__row {
//   height: 40px !important;
// }
.title_top {
  background: url(../assets/img/list/biaotibeijing.png) no-repeat;
  color: #fff;
  width: 200px /* 200/16 */;
  height: 40px /* 40/16 */ /* 50/16 */;
  font-size: 20px /* 20/16 */;
  padding-left: 30px;
  margin-top: 20px /* 20/16 */;
}
// .bg {
//   border: 1px solid #1c7aa8;
//   // border-radius: .625rem /* 10/16 */;
//   padding: 0.625rem /* 10/16 */ /* 20/16 */;
//   background-color:rgb(1, 29, 58,.9) ;
//   opacity: .9;
//   // background: url(../assets/img/list/zhongkuang.png) no-repeat;
//   // background-size: 100%;
// }
/*alert 成功弹出框样式*/
// .dialogInfo /deep/ .el-dialog {
//   background: #021c2d url(../assets/img/shouye/背景框.png) no-repeat;
//   background-size: 100% 100%;
//   padding: 10px;
//   opacity: 0.85;
//   box-sizing: border-box;
//   .el-dialog__headerbtn {
//     top: 8px !important;
//     .el-dialog__close {
//       color: #fff;
//       font-size: 16px;
//     }
//   }
//   .el-dialog__header {
//     margin: 20px 50px 0px 40px;
//     background: url(../assets/img/shouye/标题矩形.png) no-repeat;
//     background-size: 100% 100%;
//   }
//   .el-dialog__title,
//   .gailan h3,
//   .gailan1 h3 {
//     color: #2fbcfc;
//   }
//   .gailan h3,
//   .gailan1 h3 {
//     padding-left: 10px;
//   }
// }
//滚动条
::-webkit-scrollbar {
  width: 5px;
  height: 5px;
  background-color: #f5f5f5;
  border-radius: 5px;
}

/*定义滚动条轨道 内阴影+圆角*/
::-webkit-scrollbar-track {
  // -webkit-box-shadow: inset 0 0 3px #2fbcfc;
  border-radius: 5px;
  background-color: #f5f5f5;
}

/*定义滑块 内阴影+圆角*/
::-webkit-scrollbar-thumb {
  border-radius: 5px;
  // -webkit-box-shadow: inset 0 0 3px #2fbcfc;
  // background-color: #23749c;
  background-color: #7f8a96 !important;
}
.list_xia {
  padding-bottom: 20px;
  background-color: rgba(8, 38, 61, 0.6);
}

/* 修改表格的滚动条*/
/deep/ .el-table__body-wrapper::-webkit-scrollbar {
  width: 5px;
  height: 10px;
}

/* 表格滚动条的滑块*/
/deep/ .el-table__body-wrapper::-webkit-scrollbar-thumb {
  background-color: #7f8a96 !important;
  //  background-color: #7f8a96;
  border-radius: 3px;
}
.dianji {
  cursor: pointer;
}
.xinashititle {
  width: 100px;
  height: 20px;
  margin: 0 auto;
}
</style>
