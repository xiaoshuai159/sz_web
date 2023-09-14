<template>
    <div class="right_main_under">
      <Navlist style="transform: translate( 0,-0.08rem);"></Navlist>
      <!-- <div class="title_top">查询条件</div> -->
      <div class="search_select_form bg">
        <el-form
          :inline="true"
          :model="formInline"
          class="demo-form-inline"
          size="mini"
        >
        <!-- 数据来源 -->
        <el-form-item label="数据来源">
            <el-select
              style="width: 100px;"
              v-model="newdomainSimpleVo.sourceType"
              placeholder="数据来源"
              clearable
              @clear="sourceType_clearFun(newdomainSimpleVo.sourceType)"
            >
              <el-option
                v-for="item in selectData.sourceTypeData"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              >
              </el-option>
            </el-select>
          </el-form-item>
          <!-- 诈骗时间 -->
          <el-form-item label="处置时间">
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
          <el-form-item label="诈骗类型">
            <el-select
              v-model.trim="newdomainSimpleVo.fraudType"
              placeholder="诈骗类型"
              clearable
              style="width: 120px;"
              @clear="fraudType_clearFun(newdomainSimpleVo.fraudType)"
            >
              <el-option
                v-for="item in selectData.fraudypeData"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              >
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="处置状态">
            <el-select
              class="el-input-width"
              v-model.trim="newdomainSimpleVo.treatStatus"
              placeholder="状态"
              clearable
              @clear="WarningType_clearFun(newdomainSimpleVo.treatStatus)"
            >
              <el-option
                v-for="item in selectData.Status"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              >
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="协议">
            <el-select
              class="el-input-width"
              v-model.trim="newdomainSimpleVo.protocol"
              placeholder="协议"
              clearable
              @clear="WarningType_clearFun(newdomainSimpleVo.protocol)"
            >
              <el-option
                v-for="item in selectData.Protocol"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              >
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="未处置原因">
            <el-select
              class="el-input-width"
              v-model.trim="newdomainSimpleVo.rejectReason"
              placeholder="原因"
              clearable
              @clear="WarningType_clearFun(newdomainSimpleVo.rejectReason)"
            >
              <el-option
                v-for="item in selectData.Reason"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              >
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
  
        <el-table-column label="序号" type="index" width="70">
          <template slot-scope="scope">
            <span v-if="scope.row.blockTime">
              {{ scope.$index + 1 }}
            </span>
          </template>
        </el-table-column>
        <el-table-column label="处置时间" prop="blockTime" show-overflow-tooltip>
        </el-table-column>
        <el-table-column label="诈骗类型" show-overflow-tooltip width="150">
          <template slot-scope="scope">
            <!-- {{ shuzu(scope.row.fraudType) }}
             -->
            {{ scope.row.fraudType }}
          </template>
        </el-table-column>
        <el-table-column
          width="200"
          show-overflow-tooltip
          label="域名"
          prop="url"
        ></el-table-column>
        <el-table-column label="处置状态" prop="blockStatus" width="100">
        </el-table-column>
        <el-table-column label="协议" prop="protocol"> </el-table-column>
        <el-table-column
          label="数据来源"
          prop="source"
          show-overflow-tooltip
        >
        </el-table-column>
        <el-table-column
          label="未处置原因"
          prop="rejectReason"
          show-overflow-tooltip
        >
        </el-table-column>
        <el-table-column label="截图" width="100">
          <template slot-scope="scope">
            <div>
              <el-button
                type="text"
                v-if="Object.keys(scope.row).length > 0"
                size="mini"
                @click="ckxq(scope.row.id)"
              >
                查看
              </el-button>
            </div>
          </template>
        </el-table-column>
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
      <el-dialog :visible.sync="imgVisible">
        <span><img :src="dialogImageUrl" alt="图片" width="100%"/></span>
        
      </el-dialog>
      <el-dialog
        :close-on-click-modal="false"
        title="查看详情"
        :visible.sync="xiangqing"
        width="50%"
        :before-close="handleClose1"
        class="dialogInfo"
        top="5vh"
      >
        <div class="xiangqinglist">
          <div class="list1">
            <div class="list2">
              <div class="list4">
                <span class="listtitle">诈骗时间：</span>
                <span
                  class="listtitle1" :title="xintableData.fraudTime"
                  >{{ xintableData.fraudTime }}</span
                >
              </div>
              <div class="list5">
                <span class="listtitle">诈骗网站域名：</span>
                <span
                  class="listtitle1" :title="xintableData.domainName"
                  >{{ xintableData.domainName }}</span
                >
              </div>
            </div>
            <div class="list3">
              <div class="list4">
                <span class="listtitle">诈骗类型：</span>
                <span
                  class="listtitle1"  :title="xintableData.fraudType"
                  >{{ xintableData.fraudType }}</span
                >
              </div>
              <div class="list5">
                <span class="listtitle">预警等级：</span>
                <span
                  class="listtitle1" :title="xintableData.earlyGrade"
                  >{{ xintableData.earlyGrade }}</span
                >
              </div>
            </div>
          </div>
          <div class="list1">
            <div class="list2">
              <div class="list4">
                <span class="listtitle">受害人IP：</span>
                <span
                  class="listtitle1"  :title="xintableData.userIp"
                  >{{  xintableData.userIp }}</span
                >
              </div>
              <div class="list5">
                <span class="listtitle">受害人端口：</span>
                <span
                  class="listtitle1"   :title="xintableData.userPort"
                  >{{ xintableData.userPort }}</span
                >
              </div>
            </div>
            <div class="list3">
              <div class="list4">
                <span class="listtitle">受害人IP归属地：</span>
                <span class="listtitle1" :title="xintableData.userIpAscription">{{  
                   xintableData.userIpAscription
                }}</span>
              </div>
              <div class="list5">
                <span class="listtitle">手机号：</span>
                <span
                  class="listtitle1"   :title="xintableData.phone"
                  >{{  xintableData.phone }}</span
                >
              </div>
            </div>
          </div>
  
          <div class="list1">
            <div class="list2">
              <div class="list4">
                <span class="listtitle">手机号归属地：</span>
                <span
                  class="listtitle1"  :title="xintableData.phoneAddress"
                  >{{  xintableData.phoneAddress }}</span
                >
              </div>
              <div class="list5">
                <span class="listtitle">诈骗网站IP：</span>
                <span
                  class="listtitle1"   :title="xintableData.fraudIp"
                  >{{  xintableData.fraudIp }}</span
                >
              </div>
            </div>
            <div class="list3">
              <div class="list4">
                <span class="listtitle">诈骗网站端口： </span>
                <span
                  class="listtitle1"   :title="xintableData.fraudPort"
                  >{{  xintableData.fraudPort }}</span
                >
              </div>
              <div class="list5">
                <span class="listtitle">诈骗网站IP归属地：</span>
                <span
                  class="listtitle1"    :title="xintableData.fraudIpAscription"
                  >{{  xintableData.fraudIpAscription }}</span
                >
              </div>
            </div>
          </div>
          <div class="list1">
            <div class="list2">
              <div class="list4">
                <span class="listtitle">身份证：</span>
                <span
                  class="listtitle1"   :title="xintableData.idCard"
                  >{{  xintableData.idCard }}</span
                >
              </div>
              <div class="list5">
                <span class="listtitle">银行卡：</span>
                <span
                  class="listtitle1"  :title="xintableData.bankCard"
                  >{{  xintableData.bankCard }}</span
                >
              </div>
            </div>
            <div class="list3">
              <!-- <div class="list4">
                <span class="listtitle">数据源: </span>
                <span
                  class="listtitle1"
                  >{{  xintableData.dataSource}}</span
                >
              </div> -->
              <div class="list4">
                <span class="listtitle"></span>
                <span class="listtitle1"></span>
              </div>
              <div class="list5">
                <span class="listtitle"></span>
                <span class="listtitle1"></span>
              </div>
            </div>
          </div>
        </div>
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
        imgVisible:false,
        dialogImageUrl: "",
        heights: undefined,
        tableDatalist: [],
        loadingbuttext: '导出',
        loadingbut: false,
        xintableData: {
          fraudTime: '', //诈骗时间
          domainName: '', //诈骗网站域名
          fraudType: '', //诈骗类型
          earlyGrade: '', //预警等级：
          userIp: '', //受害人IP：
          userPort: '', //受害人端口：
          userIpAscription: '', //受害人IP归属地：
          phone: '', //手机号
          phoneAddress: '', //手机号归属地
          fraudIp: '', //诈骗网站IP
          fraudPort: '', //诈骗网站端口
          fraudIpAscription: '', //诈骗网站IP归属地
          idCard: '', //身份证：
          bankCard: '', //银行卡：
          dataSource: '', //数据源
        },
        xiangqing: false,
        isLoading: false,


        newdomainSimpleVo: {

          sourceType: null, //数据来源
          fraudType: null, //诈骗类型
          protocol:null,  //协议
          rejectReason:null,//未处置原因
          treatStatus:null, //处置状态
          dateValue_find: [
            dayjs().subtract(1, 'month').format('YYYY-MM-DD'),
            dayjs().format('YYYY-MM-DD'),
          ], //诈骗时间


          photo: null, //手机号
          dateValue_find1: null, //图表时间
          unit: null, //推送单位
          Reason:null
        },
        whiteSearchList1: {
          startCreateTime1: '',
          endCreateTime1: '',
        },
        whiteSearchList: {
          startCreateTime:
            dayjs().subtract(1, 'month').format('YYYY-MM-DD'),
          endCreateTime:
            dayjs().format('YYYY-MM-DD')
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
          pageNo: 1,
          pageSize: 10,
        },
        total: 0,
        totalPages: '',
        selectData: {
          Reason:[],
          Status:[],
          Protocol:[],
          sourceTypeData: [],
          WarningTypeData: [
          ],
          unitTypeData: [
          ],
          fraudypeData: [
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
    // created() {  // 7.4 测试关闭
    //   this.getTabData()
    //   // this.echartslist1()
    // },
    components: {
      Navlist: Navlist, //将别名demo 变成 组件 Demo
    },
    mounted() {
      this.yangshi()
      this.getTabData()
    },
    created(){
      this.getOptionsData()
    },
    methods: {
      async getOptionsData(){
        const promiseArr = Promise.all([await this.$http.get('/dict/protocol'),await this.$http.get('/dict/fraudType'),await this.$http.get('/dict/rejectReason'),await this.$http.get('/dict/datasource'),await this.$http.get('/dict/blockStatus'),])
        promiseArr.then((successArr)=>{
          successArr.forEach(successData=>{
            if(successData.config.url=='/dict/protocol'){
              const protocolArr = successData.data.data
              protocolArr!=null && protocolArr.forEach(protocol=>this.selectData.Protocol.push({key:protocol, value:protocol}))
            }else if(successData.config.url=='/dict/fraudType'){
              const fraudTypeArr = successData.data.data
              // console.log(fraudTypeArr);
              fraudTypeArr!=null && fraudTypeArr.forEach(fraudType=>this.selectData.fraudypeData.push({key:fraudType, value:fraudType}))
            }else if(successData.config.url=='/dict/rejectReason'){
              const rejectReasonArr = successData.data.data
              // console.log(rejectReasonArr);
              if(rejectReasonArr!=null){
                for(let i in rejectReasonArr){
                  this.selectData.Reason.push({value:i, label:rejectReasonArr[i]})
                }
              }
              // console.log(this.selectData.Reason);
              // rejectReasonArr!=null && rejectReasonArr.forEach(item=>this.selectData.Reason.push({key:item, value:item}))
            }else if(successData.config.url=='/dict/datasource'){
              const itemArr = successData.data.data
              itemArr!=null && itemArr.forEach(item=>this.selectData.sourceTypeData.push({key:item, value:item}))
            }else if(successData.config.url=='/dict/blockStatus'){
              const itemArr = successData.data.data
              // console.log(itemArr);
              if(itemArr!=null){
                for(let i in itemArr){
                  this.selectData.Status.push({value:i, label:itemArr[i]})
                }
              }
              // console.log(this.selectData.Status);
              // itemArr!=null && itemArr.forEach(item=>this.selectData.Status.push({key:item, value:item}))
            }
          })
        })
        .catch(function(e){console.log(e)})
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

          source:this.newdomainSimpleVo.sourceType,
          treatDateStart:this.newdomainSimpleVo.dateValue_find[0],
          treatDateEnd:this.newdomainSimpleVo.dateValue_find[1],
          fraudType:this.newdomainSimpleVo.fraudType,
          protocol:this.newdomainSimpleVo.protocol,
          rejectReason:this.newdomainSimpleVo.rejectReason,
          treatStatus:this.newdomainSimpleVo.treatStatus
          // earlyGrade: this.newdomainSimpleVo.Status,
          // endFraudTime: this.whiteSearchList.endCreateTime,
  
          // fraudType: this.newdomainSimpleVo.fraud,
          // newPage: this.mypageable,
          // phoneNum: this.newdomainSimpleVo.photo,
          // startFraudTime: this.whiteSearchList.startCreateTime,

        }
  
        this.$http({
          method: 'GET',
          url: '/block/export',
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
              let title = dayjs().format('YYYYMMDD') + '-处置导出.xlsx'
  
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
            console.log(err)
            this.$message.error('文件下载失败！')
            this.loadingbuttext = '导出'
            this.loadingbut = false
          })
      },

      // 初始化数据
      async getTabData() {
        let getTabDataList = {

          source:this.newdomainSimpleVo.sourceType,
          treatDateStart:this.newdomainSimpleVo.dateValue_find[0],
          treatDateEnd:this.newdomainSimpleVo.dateValue_find[1],
          fraudType:this.newdomainSimpleVo.fraudType,
          treatStatus:this.newdomainSimpleVo.treatStatus,
          protocol:this.newdomainSimpleVo.protocol,
          rejectReason:this.newdomainSimpleVo.rejectReason,
          page:this.mypageable.pageNo,
          pageSize:this.mypageable.pageSize


          // earlyGrade: this.newdomainSimpleVo.Status,
          // endFraudTime: this.whiteSearchList.endCreateTime,
          // fraudType: this.newdomainSimpleVo.fraud,
          // newPage: this.mypageable,
          // phoneNum: this.newdomainSimpleVo.photo,
          // startFraudTime: this.whiteSearchList.startCreateTime,
          
        }
        const { data: res } = await this.$http.get(
          '/block/list',
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
        this.mypageable.pageNo = 1
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
        this.newdomainSimpleVo.sourceType=null,
        this.newdomainSimpleVo.protocol=null,
        this.newdomainSimpleVo.rejectReason=null,
        this.newdomainSimpleVo.treatStatus=null,
        this.mypageable.pageNum = 1,
        this.mypageable.pageSize = 10
        this.getTabData()
        // this.newdomainSimpleVo = {
        //   photo: null,
        //   dateValue_find: null,
        //   sourceType: null,
        //   // sourceType: null,
        //   fraud: null,
        //   unit: null,
        //   Status: null,
        // }
        // this.whiteSearchList = {
        //   startCreateTime: null,
        //   endCreateTime: null,
        // }
        // this.mypageable.pageNo = 1
        // this.getTabData()
      },
      // 分页
      handleSizeChange(val) {
        // console.log(val);
        this.mypageable.pageSize = val
        this.getTabData()
      },
      handleCurrentChange(val) {
        // console.log(val, 111);
  
        this.mypageable.pageNo = val
  
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
      ckxq(val) {
        this.$http({
          url:'/block/snapshot',
          method:'get',
          responseType: 'blob',	
          params:{ id:val }
        }).then(res=>{
          let url = window.URL.createObjectURL(res.data);
          this.dialogImageUrl = url
          this.imgVisible = true
        })
        // const res = await this.$http.get('/block/snapshot', {
        //   params: {
        //     id: val,
        //   },
        // })
        // const blob = new Blob([res.data], { type: res.headers['content-type'] });
        // this.dialogImageUrl = window.URL.createObjectURL(blob);
        // this.imgVisible = true
      },
      handleClose1() {
        this.xiangqing = false
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