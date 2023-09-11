<template>
    <div class="right_main_under">
      <Navlist></Navlist>
      <!-- <div class="title_top">查询条件</div> -->
      <div class="search_select_form bg">
        <el-form
          :inline="true"
          :model="formInline"
          class="demo-form-inline"
          size="mini"
        >
          <!-- 诈骗时间 -->
          <el-form-item label="时间">
            <el-date-picker
              v-model="newdomainSimpleVo.dateValue_find"
              type="daterange"
              :change="dataCreate_change(newdomainSimpleVo.dateValue_find)"
              range-separator="至"
              start-placeholder="开始日期"
              end-placeholder="结束日期"
              value-format="yyyy-MM-dd"
            >
            </el-date-picker>
          </el-form-item>
          <!-- 诈骗类型 -->
          <el-form-item label="域名">
            <el-input
              v-model.trim="newdomainSimpleVo.url"
              placeholder="输入查询域名"
            >
            </el-input>
          </el-form-item>
          <el-form-item label="诈骗类型">
            <el-select v-model.trim="newdomainSimpleVo.fraudType" clearable placeholder="诈骗类型" style="width: 120px;">
              <el-option
                v-for="item in newdomainSimpleVo.fraudTypeOptions"
                :key="item.value"
                :label="item.label"
                :value="item.value">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item>
            <el-button
              class="el-button-daochu"
              type="primary"
              size="mini"
              @click.native="searchTabData"
              v-preventReClick
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
              @click.native.stop="put"
              :loading="loadingbut"
              >{{ loadingbuttext }}</el-button
            >
            <el-button
              class="el-button-daochu"
              type="primary"
              size="mini"
              @click.native="addDialog"
              >新增</el-button
            >
            <!-- v-if="getRole1('downloadWarning')" -->
          </el-form-item>
        </el-form>
      </div>
      <!-- <div class="list_xia"> -->
      <!-- //列表 -->
      <!-- <div class="title_top">查询结果</div> -->
      <el-table
        :row-class-name="tableRowClassName"
        :row-key="getRowKeys"
        ref="multipleTable"
        :data="tableData"
        style="width: 100%"
        :height="heights"
        size="mini"
        class="tableStyle"
        id="onetable"
        @selection-change="handleSelectionChange"
      >
        <!-- max-height="600px" -->
  
        <el-table-column type="selection" :reserve-selection="true" width="55">
        </el-table-column>
        <el-table-column label="日期" prop="createTime" show-overflow-tooltip>
        </el-table-column>

        <el-table-column
          width="200"
          show-overflow-tooltip
          label="域名"
          prop="url"
        ></el-table-column>
        <el-table-column label="类型" show-overflow-tooltip width="150">
          <template slot-scope="scope">
            <!-- {{ shuzu(scope.row.fraudType) }}
             -->
            {{ scope.row.fraudType }}
          </template>
        </el-table-column>
        <el-table-column
          label="原因"
          prop="reason"
          show-overflow-tooltip
        >
        </el-table-column>
        <el-table-column
          label="备注"
          prop="remark"
          show-overflow-tooltip
        >
        </el-table-column>
        <!-- <el-table-column label="操作" width="100">
          <template slot-scope="scope">
            <div>
              <el-button
                type="text"
                v-if="Object.keys(scope.row).length > 0"
                size="mini"
                @click="ckxq(scope.row.id)"
              >
                查看详情
              </el-button>
            </div>
          </template>
        </el-table-column> -->
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
      <!-- </div>  -->
  
      <!-- ====================== -->
  
      <el-dialog
        :close-on-click-modal="false"
        title="新 增"
        :visible.sync="dialog"
        width="35%"
        class="dialogInfo"
        
      >
      <el-form
        :rules="rules"
        ref="dialogInfo"
        label-width="90px"
        :inline="true"
        :model="dialogInfo"
        class="demo-form-inline search_select_form"
        size="mini"
      >
        <el-form-item label="域名:" prop="dialog_domain">
          <el-input
            v-model.trim="dialogInfo.dialog_domain"
            placeholder="例：https://www.****.com"
          ></el-input>
        </el-form-item>
        <el-form-item label="诈骗类型:" prop="dialog_fraudType">
          <el-select
            v-model="dialogInfo.dialog_fraudType"
            placeholder="诈骗类型"
          >
            <el-option
              v-for="item in newdomainSimpleVo.fraudTypeOptions"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="原因:" prop="dialog_Reason">
          <el-input
            v-model.trim="dialogInfo.dialog_Reason"
            placeholder="请输入原因"
          ></el-input>
        </el-form-item>
        <el-form-item label="备注:" prop="dialog_Remark">
          <el-input
            v-model.trim="dialogInfo.dialog_Remark"
            placeholder="请输入备注"
          ></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button
          @click="submitForm('dialogInfo')"
          type="primary"
          size="mini"
          class="el-button-xitongerr"
          >添 加</el-button
        >
        <el-button
          class="el-button-xitongup"
          type="primary"
          @click="quxiao2"
          size="mini"
          >取 消</el-button
        >
      </span>
      </el-dialog>
    </div>
  </template>
  
  <script>
  import Navlist from '@/components/hearderdongtainav.vue'
  import getRole from '@/utils/promission.js'
  import yujingweifen from '@/utils/yujingweifenlei.js'
  import dayjs from 'dayjs'
  export default {
    data() {
      return {
        rules:{
          dialog_domain:[{ required: true, message: '请输入域名', trigger: 'blur' }],
          dialog_fraudType:[{ required: true, message: '请选择诈骗类型', trigger: 'blur' }],
          dialog_Reason:[{ required: true, message: '请输入原因', trigger: 'blur' }],
          dialog_Remark:[{ required: true, message: '请输入备注', trigger: 'blur' }],
        },
        dialogInfo:{
          dialog_domain:null,
          dialog_fraudType:null,
          dialog_Reason:null,
          dialog_Remark:null
        },
        heights: undefined,
        tableDatalist: [],
        loadingbuttext: '导出',
        loadingbut: false,
        xiangqing: false,
        isLoading: false,
        dialog:false,

        newdomainSimpleVo: {
          fraudType: null,
          url:null,
          dateValue_find: [
            dayjs().subtract(1, 'month').format('YYYY-MM-DD'),
            dayjs().format('YYYY-MM-DD') 
          ], //诈骗时间



          fraudTypeOptions:[],
          photo: null, //手机号
          unit: null, //推送单位
          sourceType: null, //数据来源
          Status: null,
          fraud: null, //诈骗类型
          dateValue_find1: null, //图表时间
          Reason:null
        },
        whiteSearchList1: {
          startCreateTime1: '',
          endCreateTime1: '',
        },
        whiteSearchList: {
          startCreateTime:
            dayjs().subtract(1, 'month').format('YYYY-MM-DD') +
            ' ' +
            '00:' +
            '00:' +
            '00',
          endCreateTime:
            dayjs().format('YYYY-MM-DD') + ' ' + '23:' + '59:' + '59',
        },
        formInline: {
          user: '',
          region: '',
        },
        tableData: [
          // {
          //   fraudTime: 'we',
          // },
        ],
        mypageable: {
          pageNum: 1,
          pageSize: 10,
        },
        total: 1,
        totalPages: '',
        selectData: {
          Reason:[],
          Status:[],
          Protocol:[],
          sourceTypeData: [{ value: '长安通信', label: '长安通信' }],
          WarningTypeData: [
            { value: '高危', label: '高危' },
            { value: '中危', label: '中危' },
            { value: '低危', label: '低危' },
          ],
          unitTypeData: [
            {
              value: '四川省成都市公安局刑警支队',
              label: '四川省成都市公安局刑警支队',
            },
          ],
          fraudypeData: [
            { value: '贷款', label: '贷款' },
            { value: '理财', label: '理财' },
            { value: '冒充客服', label: '冒充客服' },
            { value: '杀猪盘', label: '杀猪盘' },
            // { value: '冒充公检法', label: '冒充公检法' },
            { value: '冒充公检法', label: '冒充公检法' },
            { value: '刷单', label: '刷单' },
            { value: '虚假购物', label: '虚假购物' },
                 { value: '其他类型诈骗', label: '其他类型诈骗' },
          ],
        },
        newqutest: [],
        newqutest1: [],
        newqutest2: [],
        newqutest3: [],
        newzhutest1: [],
        newzhutest2: [],
        newzhutest3: [],
        newzhutest4: [],
        newwarningStatisticsTypeList: [],
      }
    },
    created() {  // 7.4 测试关闭
      this.getOptionsData()
      // this.echartslist1()
    },
    components: {
      Navlist: Navlist, //将别名demo 变成 组件 Demo
    },
    mounted() {
      this.yangshi()
      this.getTabData()
    },
    methods: {
      submitForm(formName) {
        let that = this
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.tianjia()

            this.dialogVisible = false
            this.$nextTick(() => {
              that.$refs['dialogInfo'].clearValidate()
            })
          } else {
            // console.log('error submit!!');
            return false
          }
      })
    },
    async tianjia() {
      let list = {
        url: this.dialogInfo.dialog_domain,
        fraudType: this.dialogInfo.dialog_fraudType,
        reason:this.dialogInfo.dialog_Reason,
        remark:this.dialogInfo.dialog_Remark
      }
      const { data: res } = await this.$http.post('/special/add', list)
      if (res.code == 200) {
        this.$message(res.message)
        this.getTabData()
        this.dialog = false
      } else {
        this.$message(res.message)
        this.dialog = false
      }
    },
    quxiao2() {
      let that = this
      this.$nextTick(() => {
        that.$refs['dialogInfo'].clearValidate()
      })
      this.dialog = false
    },
      async getOptionsData(){
        const {data:res} = await this.$http.get('/dict/fraudType')
        if(res.code == 200){
          const fraudTypeArr = res.data
          fraudTypeArr!=null && fraudTypeArr.forEach(fraudType=>this.newdomainSimpleVo.fraudTypeOptions.push({key:fraudType, value:fraudType}))
        }else{
          this.$message(res.data)
        }
      },
      yangshi() {
        this.heights =
          window.innerHeight - this.$refs.multipleTable.$el.offsetTop - 270
        document.querySelector('#onetable .el-table__body tbody').style.height =
          (this.heights / 10) * 9 + 'px' // var rows = document.querySelectorAll('#onetable .el-table__row ') // // console.log( document.querySelectorAll('#onetable .el-table__row ')); // rows.forEach((item) => { //   item.style.height = Math.floor(this.heights / 11) + 'px' // })
  
        document.querySelector('#onetable .has-gutter tr ').style.height =
          Math.floor(this.heights / 11) + 'px' // // 监听浏览器高度变化，修改表格高度
  
        window.onresize = () => {
          this.heights =
            window.innerHeight - this.$refs.multipleTable.$el.offsetTop - 270
  
          document.querySelector('#onetable .el-table__body tbody').style.height =
            (this.heights / 10) * 9 + 'px' // var rows = document.querySelectorAll('#onetable .el-table__row ') // rows.for
  
          document.querySelector('#onetable .has-gutter tr ').style.height =
            Math.floor(this.heights / 11) + 'px'
        }
      },
  
      getRole1(data) {
        return getRole(data)
        // console.log( getRole(data));
      },
      handleSelectionChange(val) {
        this.tableDatalist = val
      },
      //下载
  
      put() {
        this.loadingbuttext = '...加载中'
        this.loadingbut = true
        let getTabDataList = {
          startDay:this.newdomainSimpleVo.dateValue_find[0],
          endDay:this.newdomainSimpleVo.dateValue_find[1],
          fraudType:this.newdomainSimpleVo.fraudType,
          url:this.newdomainSimpleVo.url
          // earlyGrade: this.newdomainSimpleVo.Status,
          // endFraudTime: this.whiteSearchList.endCreateTime,
  
          // fraudType: this.newdomainSimpleVo.fraud,
          // newPage: this.mypageable,
          // phoneNum: this.newdomainSimpleVo.photo,
          // startFraudTime: this.whiteSearchList.startCreateTime,
        }
  
        this.$http({
          method: 'GET',
          url: '/special/export',
          responseType: 'blob',
          params: getTabDataList,
        })
          .then((res) => {
            // const blob = new Blob([res.data], {
            //   type: 'application/vnd.ms-excel',
            // })
            //        const url = window.URL.createObjectURL(blob)
            // window.open(url, '_blank')
            let that = this
            let blob = res.data
            if (blob.type == 'application/json') {
              const reader = new FileReader()
  
              reader.onload = function () {
                // const { msg } = JSON.parse(reader.result) //此处的msg就是后端返回的msg内容
  
                that.$message('下载文件失败')
                that.loadingbuttext = '导出'
                that.loadingbut = false
              }
              reader.readAsText(blob)
            } else {
              let title = dayjs().format('YYYYMMDD') + '-特办导出.xlsx'
  
              let binaryData = []
              binaryData.push(blob)
              let url = window.URL.createObjectURL(new Blob(binaryData), {
                type: 'application/vnd.ms-excel',
              })
  
              // 创建一个下载标签<a>
              const aLink = document.createElement('a')
              aLink.href = url
  
              // 2.直接使用自定义文件名,设置下载文件名称
              aLink.setAttribute('download', title)
              document.body.appendChild(aLink)
  
              // 模拟点击下载
              aLink.click()
  
              this.loadingbuttext = '导出'
              this.loadingbut = false
              // 移除改下载标签
              document.body.removeChild(aLink)
            }
  
            // console.log(title);
          })
          .catch((err) => {
            // console.log(err)
            this.$message.error('文件下载失败！')
            this.loadingbuttext = '导出'
            this.loadingbut = false
          })
      },
      // async put123() {
      //   this.loadingbuttext = '...加载中'
      //   this.loadingbut = true
      //   let arr = []
      //   this.tableDatalist.forEach((item) => {
      //     arr.push(item.id)
      //   })
      //   let putlist = {
      //     warningSimpleVo: {
      //       // pushUnit: this.newdomainSimpleVo.unit,
      //       dataSource: this.newdomainSimpleVo.sourceType,
      //       earlyGrade: this.newdomainSimpleVo.Warning,
      //       fraudType: this.newdomainSimpleVo.fraud,
      //     },
      //     warningTimeVo: {
      //       startFraudTime: this.whiteSearchList.startCreateTime,
      //       endFraudTime: this.whiteSearchList.endCreateTime,
      //     },
      //     idList: arr,
      //   }
  
      //   const { data: res } = await this.$http.post(
      //     '/warning/downloadWarning',
      //     putlist
      //   )
      //   if (res.code == 200) {
      //     this.loadingbuttext = '导出'
      //     this.loadingbut = false
      //     let newurl = res.expandData.url
      //     let eleLink = document.createElement('a')
      //     eleLink.download = name
      //     // const down = window.location.origin
      //     // eleLink.href = "http://172.31.1.61:8080" + newurl;
      //     // const down = window.location.origin
      //     eleLink.href = newurl
  
      //     // console.log(eleLink);
      //     eleLink.click()
      //     eleLink.remove()
      //     if (this.tableDatalist.length > 0) {
      //       this.$refs.multipleTable.clearSelection()
      //     }
      //   } else {
      //     this.$message(res.message)
      //   }
      // },
  
     addDialog(){
      this.dialog = true
      this.dialogInfo.dialog_domain = null
        this.dialogInfo.dialog_fraudType = null
        this.dialogInfo.dialog_Reason = null
        this.dialogInfo.dialog_Remark = null
     },
      // 初始化数据
      async getTabData() {
        let getTabDataList = {

          startDay:this.newdomainSimpleVo.dateValue_find[0],
          endDay:this.newdomainSimpleVo.dateValue_find[1],
          fraudType: this.newdomainSimpleVo.fraudType,
          url:this.newdomainSimpleVo.url,
          page:this.mypageable.pageNum,
          pageSize:this.mypageable.pageSize


          // earlyGrade: this.newdomainSimpleVo.Status,
          // endFraudTime: this.whiteSearchList.endCreateTime,
  
          // newPage: this.mypageable,
          // phoneNum: this.newdomainSimpleVo.photo,
          // startFraudTime: this.whiteSearchList.startCreateTime,
        }
        const { data: res } = await this.$http.get(
          '/special/list',
          {params:getTabDataList}
        )
        if (res.code == 200) {
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
      },
  
      //查询
      searchTabData() {
        this.mypageable.pageNum = 1
        this.getTabData()
        // let getTabDataList = {
        //   warningSimpleVo: {
        //     pushUnit: this.newdomainSimpleVo.unit,
        //     dataSource: this.newdomainSimpleVo.sourceType,
        //     earlyGrade: this.newdomainSimpleVo.Warning,
        //     fraudType: this.newdomainSimpleVo.fraud,
        //   },
        //   warningTimeVo: {
        //     startFraudTime: this.whiteSearchList.startCreateTime,
        //     endFraudTime: this.whiteSearchList.endCreateTime,
        //   },
        //   mypageable: this.mypageable,
        // };
  
        // const { data: res } = await this.$http.post(
        //   "/warning/getWarning",
        //   getTabDataList
        // );
        // if (res.code == 200) {
        //   this.mypageable.pageNum = 1;
        //   // if (res.data.content.length > 0) {
        //   // console.log(res.data.content);
        //   this.tableData = res.data.content;
        //   this.total = res.data.totalElements;
        //   this.totalPages = res.data.totalPages;
  
        //   // } else {
        //   // this.mypageable.pageNum = 1;
        //   // this.mypageable.pageSize = 10;
        //   // this.getTabData();
        //   // }
        // } else {
        //   this.$message("无数据");
        //   this.mypageable.pageNum = 1;
        //   this.mypageable.pageSize = 10;
        //   this.getTabData();
        //   this.resetFun();
        // }
      },
  
      dataCreate_change1(val) {
        if (val && val != '') {
          this.whiteSearchList1.startCreateTime1 = val[0]
          this.whiteSearchList1.endCreateTime1 = val[1]
        } else {
          this.whiteSearchList1.startCreateTime1 = null
          this.whiteSearchList1.endCreateTime1 = null
        }
      },
      //诈骗时间
      dataCreate_change(val) {
        if (val && val != '') {
          this.whiteSearchList.startCreateTime = val[0]
          this.whiteSearchList.endCreateTime = val[1]
        } else {
          this.whiteSearchList.startCreateTime = null
  
          this.whiteSearchList.endCreateTime = null
        }
      },
      sourceType_clearFun(val) {
        if (val == '') {
          this.newdomainSimpleVo.sourceType = null
        }
      },
      unitType_clearFun(val) {
        if (val == '') {
          this.newdomainSimpleVo.unit = null
        }
      },
      WarningType_clearFun(val) {
        if (val == '') {
          this.newdomainSimpleVo.Status = null
        }
      },
      fraudType_clearFun(val) {
        if (val == '') {
          this.newdomainSimpleVo.fraud = null
        }
      },
      //row-keys
      modelType1_photo(val) {
        if (val == '') {
          this.newdomainSimpleVo.uploadPerson = null
        }
      },
      //重置方法
      resetFun() {

        this.newdomainSimpleVo.dateValue_find=[ dayjs().subtract(1, 'month').format('YYYY-MM-DD'), dayjs().format('YYYY-MM-DD')],
        this.newdomainSimpleVo.fraudType=null,
        this.newdomainSimpleVo.url=null,
        this.mypageable.pageNum = 1
        this.mypageable.pageSize = 10
        this.getTabData()



        // this.newdomainSimpleVo = {

        // //   photo: null,
        // //   dateValue_find: null,
        // //   sourceType: null,
        // //   // sourceType: null,
        // //   fraud: null,
        // //   unit: null,
        // //   Status: null,
        // // }
        // // this.whiteSearchList = {
        // //   startCreateTime: null,
        // //   endCreateTime: null,
        // // }
        // // this.mypageable.pageNum = 1
      
      },
      // 分页
      handleSizeChange(val) {
        // console.log(val);
        this.mypageable.pageSize = val
        this.getTabData()
      },
      handleCurrentChange(val) {
        // console.log(val, 111);
  
        this.mypageable.pageNum = val
  
        // console.log( this.mypageable.pageNum);
        this.getTabData()
      },
  
      // handleSelectionChange(val) {
      //   this.tableDatalist = val;
      // },
      getRowKeys(row) {
        return row.id
      },
  
      // 数据来源
      // shuzu(val){
      //   if(val=='1'){
      //     return '贷款'
      //   }
      //    else if(val=='2'){
      //     return '理财'
      //   }
      //   else  if(val=='3'){
      //     return '冒充客服'
      //   }
      //   else  if(val=='4'){
      //     return '杀猪盘'
      //   }
      //   else  if(val=='5'){
      //     return '冒充公检法'
      //   }
      //   else  if(val=='6'){
      //     return '刷单'
      //   }
      // }
      // shuzu(val) {
      //   if (val == '贷款') {
      //     return '贷款'
      //   } else if (val == '理财') {
      //     return '理财'
      //   } else if (val == '冒充电商客服') {
      //     return '冒充电商客服'
      //   } else if (val == '杀猪盘') {
      //     return '杀猪盘'
      //   } else if (val == '冒充公检法') {
      //     return '冒充公检法'
      //   } else if (val == '刷单') {
      //     return '刷单'
      //   } else if (val == '博彩/投资/交友') {
      //     return '博彩/投资/交友'
      //   }
      // },
      tableRowClassName({ rowIndex }) {
        if (rowIndex % 2 === 0) {
          return 'warning-row'
        } else if (rowIndex % 2 === 1) {
          return 'success-row'
        }
        return ''
      },
      async ckxq(val) {
        // console.log(val);
  
        const { data: res } = await this.$http.get('/warning_ca/tailDetail', {
          params: {
            id: val.toString(),
          },
        })
        if (res.code == 200) {
          this.xintableData = res.data
  
          // console.log(this.xintableData);
          this.xiangqing = true
        }
      },
      handleClose1() {
        console.log('执行了handleClose1');
        this.dialogInfo.dialog_domain = null
        this.dialogInfo.dialog_fraudType = null
        this.dialogInfo.dialog_Reason = null
        this.dialogInfo.dialog_Remark = null
      },
    },
  }
  </script>
  
  <style scoped lang='less'>
  // 按钮hover
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
  .right_main_under /deep/ .el-button-xitongerr:focus,
.right_main_under /deep/ .el-button-xitongerr:hover {
  background: url(../assets/img/shouye/确定按钮.png) no-repeat;
  background-size: cover;
  border: none;
  color: #25c0fd;
}
.right_main_under /deep/ .el-button-xitongup:focus,
.right_main_under /deep/ .el-button-xitongup:hover {
  background: url(../assets/img/shouye/取消按钮.png) no-repeat;
  background-size: cover;
  border: none;
}
  .el-table::before {
    height: 0;
    /* // 将高度修改为0 */
  }
  // 点击变黑
  /deep/ .el-table__fixed-right::before,
  .el-table__fixed::before {
    background-color: #192d45;
  }
  /deep/.el-table--enable-row-hover .el-table__body tr:hover > td {
    background-color: transparent;
  }
  .tubiao {
    width: 100% /* 1558/16 */;
    height: 12.5rem /* 200/16 */ /* 300/16 */;
    margin-bottom: 2rem;
    box-sizing: border-box;
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
    border-radius: 10px;
  }
  .right {
    width: 40%;
    height: 10rem;
    background-color: #07293f;
    // float: right;
    // padding: 1.25rem /* 20/16 */ /* 50/16 */ 3.125rem /* 100/16 */;
    box-sizing: border-box;
    border-radius: 10px;
  }
  #bar_qx {
    height: 10rem /* 240/16 */ /* 260/16 */ /* 200/16 */;
    padding-top: 0.625rem /* 10/16 */;
    margin-left: 0.625rem /* 10/16 */ /* 20/16 */;
  }
  #bar_zz {
    height: 10rem /* 200/16 */;
    // margin-left: 1.875rem /* 30/16 */ /* 20/16 */;
    padding-top: 0.625rem /* 10/16 */;
    margin-left: 0.625rem;
  }
  .qwqw {
    margin-top: 10px;
    border: 1px solid transparent !important;
  }
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
  
  .xiangqinglist {
    // background-color: red;
    width: 100%;
    height: 600px;
    box-sizing: border-box;
    .list1 {
      // flex-flow: column;
      height: 120px;
      width: 100%;
      // background-color: #fff;
      margin-top: 10px;
      margin-bottom: 5px;
      .list2 {
        width: 100%;
        height: 50%;
        background: url(../assets/img/shouye/矩形.png) no-repeat;
        background-size: 100% 100%;
      }
      .list3 {
        width: 100%;
        height: 50%;
        // background-color: blue;
        background: url(../assets/img/shouye/边框.png) no-repeat;
        background-size: 100% 100%;
      }
      .list4 {
        width: 57%;
        height: 100%;
        float: left;
        align-items: center;
        display: -webkit-flex;
      }
      .list5 {
        width: 43%;
        height: 100%;
        float: left;
        align-items: center;
        display: -webkit-flex;
      }
      .listtitle {
        font-size: 12px;
        color: #94a4a7;
        padding-left: 20px;
        box-sizing: border-box;
        width: 130px;
      }
      .listtitle1 {
        flex: 1;
        font-size: 12px;
        color: #fff;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
      }
    }
  }
  
  // 弹窗
  .dialogInfo /deep/ .el-dialog {
    background: #021c2d url(../assets/img/shouye/背景框.png) no-repeat;
    background-size: 100% 100%;
    padding: 10px;
    opacity: 0.9;
    box-sizing: border-box;
    .el-dialog__headerbtn {
      top: 5px !important;
      right: 10px !important;
      .el-dialog__close {
        color: #fff;
        font-size: 14px;
      }
    }
    .el-dialog__header {
      margin: 20px 20px 0px 20px;
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
  .list_xia {
    padding-bottom: 20px;
    background-color: rgba(8, 38, 61, 0.6);
  }
  .bottom {
    width: 100%;
    height: 40px /* 60/16 */ /* 40/16 */;
    background-color: rgba(11, 48, 78, 0.5);
    padding-bottom: 1%;
  }
  </style>