<template>
  <div>
    <el-row :gutter="10">
      <el-col :span="17">
        <div element-loading-text="拼命加载中"
             element-loading-spinner="el-icon-loading"
             element-loading-background="rgba(0, 0, 0,0.8)"
             v-loading="loading">

          <Gd_map @transferID="getChange"
                  :openmsg="openmsg"
                  ref="myChild"> </Gd_map>
        </div>
      </el-col>
      <el-col :span="7">

        <div class="picture_header">
          <div class="picture_title">
            <span @click="click1()"
                  style="cursor:pointer">消息列表</span>
            <el-divider direction="vertical"></el-divider>
            <span @click="click2()"
                  style="cursor:pointer">上报病人列表</span>
          </div>
        </div>

        <div class="picture_body22"
             id="myexample">
          <div v-if="card=='first'">
            <div class="demo-table1">

              <el-table :data="tablemsg"
                        height="170"
                        :cell-style="{'font-size':'16px',}"
                        :show-header="false">
                <el-table-column type="expand">
                  <template slot-scope="props">
                    <span v-for="i in props.row.detail"
                          style="margin-left:20px;">
                      {{i}}</br>
                    </span>
                  </template>
                </el-table-column>
                <el-table-column prop="title">
                </el-table-column>
              </el-table>
            </div>

          </div>
          <div v-else>

            <el-table :data="ReportedPatientList"
                      size="small"
                      max-height="170"
                      :cell-style="{padding:0,'background-color': '#c6e2ff','color': '#303133','font-size':'16px'}"
                      @row-click="rowClick"
                      :header-cell-style="{'background-color': '#a0cfff','color': '#303133', 'text-align':'center',padding:0,'font-size':'16px'}">
              <el-table-column prop="PatientId"
                               label="编号">

              </el-table-column>
              <el-table-column prop="Name"
                               label="姓名">

              </el-table-column>

              <el-table-column prop="StatusName"
                               label="状态">

              </el-table-column>

            </el-table>

          </div>

        </div>
        <el-row style="margin-top:10px;">

          <div class="groupbox-boarder">
            <el-row style="padding:5px;">

              <el-button @click="generate()"
                         style="font-size:20px; padding: 8px 16px;">
                生成推荐方案
              </el-button>

              <el-button @click="cancel()"
                         style="font-size:20px; padding: 8px 16px;">
                取消闪烁
              </el-button>

            </el-row>

          </div>
        </el-row>
        <el-row style="margin-top:10px;">
          <div class="groupbox-boarder">

            <el-row style="padding:5px;">
              <el-button @click="openass()"
                         style="font-size:20px; padding: 8px 16px;">
                现场组
              </el-button>
              <el-button @click="opencar()"
                         style="font-size:20px; padding: 8px 16px;">
                车辆组
              </el-button>
              <el-button @click="openhos()"
                         style="font-size:20px; padding: 8px 16px;">
                医院组
              </el-button>
              <el-button @click="specialist()"
                         style="font-size:20px; padding: 8px 16px;">
                专家组
              </el-button>
            </el-row>

          </div>
        </el-row>
        <el-row style="margin-top:10px;">
          <div v-if="showtype==='overall'">
            <overall></overall>
          </div>
          <div v-else-if="showtype===  'grouplist'">
            <div v-if="grouptype==='car'">

              <div class="picture_header">
                <div class="picture_title">
                  车辆列表
                  <span style="float:right;cursor:pointer"
                        @click="showtype='overall'">返回</span>
                </div>
              </div>
              <div class="picture_bodyy">
                <div v-for="item in carlist"
                     style="background-color: #404a59;cursor:pointer ">
                  <div class="text-group"
                       @click="choosecar(item.CarNo)">
                    <!-- {{item.CarName}}</br> -->
                    <span class="card-title">{{item.CarName}}</span></br>
                    {{item.Description}}</br>
                    车牌号: {{item.CarId}}
                    &nbsp;&nbsp;
                    状态：{{item.CarStatus}}</br>
                    车辆负责人:
                    {{item.CarManager}}&nbsp;&nbsp;
                    联系方式: {{item.phone}}

                    <hr>
                  </div>
                </div>
              </div>

            </div>

            <div v-else-if="grouptype==='hos'">
              <div class="picture_header">
                <div class="picture_title">
                  医院列表
                  <span style="float:right;cursor:pointer"
                        @click="showtype='overall'">返回</span>
                </div>
              </div>
              <div class="picture_bodyy">
                <div v-for="item in hoslist"
                     style="background-color: #404a59; cursor:pointer">
                  <div class="text-group"
                       @click="choosehos(item.OrganizationCode)">
                    <span class="card-title">{{item.OrganizationName}}</span></br>
                    <mt-badge v-show="item.XiongtongTag">胸痛</mt-badge>
                    <mt-badge v-show="item.GanranTag"> 感染</mt-badge>
                    <mt-badge v-show="item.ZhongduTag">中毒</mt-badge>
                    <mt-badge v-show="item.CuzhongTag">卒中</mt-badge>
                    <mt-badge v-show="item.ChuangshangTag">创伤</mt-badge>
                    <mt-badge v-show="item.HefusheTag">核辐射</mt-badge>
                    <mt-badge v-show="item.YunchanTag">孕产</mt-badge>
                    <mt-badge v-show="item.ErtongTag">儿童</mt-badge>
                    <mt-badge v-show="item.ErchuangTag">儿创</mt-badge>
                    <mt-badge v-show="item.JingshenTag">精神</mt-badge>
                    <mt-badge v-show="item.XinliTag">心理</mt-badge>
                    <mt-badge v-show="item.ChuanranTag">传染</mt-badge>
                    </br>
                    位置：{{item.LocationDescription}}</br>
                    医院负责人：{{item.realManager}}&nbsp;&nbsp;联系方式：{{item.phone}}
                    <hr>
                  </div>
                </div>
              </div>

            </div>
            <div v-else-if="grouptype==='ass'">
              <div class="picture_header">
                <div class="picture_title">
                  会场列表
                  <span style="float:right;cursor:pointer"
                        @click="showtype='overall'">返回</span>
                </div>
              </div>
              <div class="picture_bodyy">
                <div v-for="item in asslist"
                     style="background-color: #404a59;cursor:pointer">
                  <div class="text-group"
                       @click="chooseass(item.LocationNo)">
                    <!-- {{item.LocationName}} </br> -->
                    <span class="card-title">{{item.LocationName}}</span></br>

                    位置: <span style="color:#d3b29b;font-size:20px;">{{item.Description}}</span></br>
                    会场负责人: {{item.Manager}} &nbsp;&nbsp; 联系方式: {{item.phone}}
                    <hr>

                  </div>
                </div>
              </div>

            </div>
            <div v-else>
              <div class="picture_header">
                <div class="picture_title">
                  专家列表
                  <span style="float:right;cursor:pointer"
                        @click="showtype='overall'">返回</span>
                </div>
              </div>
              <div class="picture_bodyy">
                <div v-for="item in expertlist"
                     style="background-color: #404a59;cursor:pointer">

                  <div class="text-group">
                    <el-row>
                      <el-col :span="20">

                        <span class="card-title">{{item.Name}}</span></br>
                        {{item.DepartmentCode}} {{item.TitleCode}} &nbsp;&nbsp;
                        专长：{{item.Specialty}}
                      </el-col>
                      <el-col :span="4">
                        <i style="font-size: 30px; color:#7eb37e"
                           class="el-icon-phone"
                           @click="sendcall(item.GroupNo)">
                        </i></br>
                        <font size="1"> 视频通话</font>
                      </el-col>
                    </el-row>

                    <hr>
                  </div>
                </div>
              </div>

            </div>

          </div>
          <div v-else>
            <component :is="chartVue"
                       :mid="eid"
                       v-if="isRouterAlive"
                       @comVisible="changecomVisible">
            </component>
          </div>

          <!-- <div v-if="overallVisiable">
            <overall></overall>
          </div>

          <div v-else>
            <component :is="chartVue"
                       :mid="eid"
                       v-if="isRouterAlive"
                       @comVisible="changecomVisible">
            </component>
          </div> -->
        </el-row>
      </el-col>
    </el-row>
    <el-dialog :visible.sync="showPatinfo"
               :before-close="handleClose"
               width="40%"
               style="text-align:left;">
      <el-row style="font-size:16px;">
        <span class="text-title">姓名：</span> {{perinfo.Name}} &nbsp;&nbsp;<span class="text-title">性别：</span>{{perinfo.Gender}}&nbsp;&nbsp;<span class="text-title">年龄：</span>{{perinfo.Age}} &nbsp;&nbsp;
        <el-button style="float:right"
                   size="mini"
                   @click="toolipVisible=true">查看真实姓名</el-button>
        <el-button style="float:right"
                   size="mini"
                   @click="refresh(perinfo.PatientId)">刷新</el-button>
      </el-row>
      <el-row style="font-size:16px;">
        <span class="text-title"> 状态：</span> {{perinfo.StatusName}} &nbsp;&nbsp;
        <span class="text-title">分级：</span> {{perinfo.Classification}} &nbsp;&nbsp;
        <span class="text-title">诊断：</span>{{perinfo.Diagnose}} </br>
        <span class="text-title">现场：</span> {{perinfo.EmergencyGroupName}}&nbsp;&nbsp;
        <span class="text-title">分配车辆：</span> {{perinfo.CarName}} &nbsp;&nbsp;
        <span class="text-title"> 分配医院：</span> {{perinfo.OrganizationName}}
      </el-row>
      <el-dialog title="提示"
                 :visible.sync="toolipVisible"
                 width="30%"
                 append-to-body>
        <span>确认要查看病人真名吗？</span>
        <span slot="footer"
              class="dialog-footer">
          <el-button @click="toolipVisible = false">取 消</el-button>
          <el-button type="primary"
                     @click="getMemberInfo(perinfo.MemberId)">确 定</el-button>
        </span>
      </el-dialog>

      </br>

      <div>
        <div>
          <mt-navbar v-model="selected"
                     style="backgroundColor:#0070a8;color:white;font-size:10px;">
            <mt-tab-item id="病史">

              <div style="font-size:16px">病史</div>
            </mt-tab-item>
            <mt-tab-item id="体征">

              <div style="font-size:16px">体征</div>
            </mt-tab-item>
            <mt-tab-item id="处置">

              <div style="font-size:16px">处置</div>
            </mt-tab-item>
          </mt-navbar>
        </div>
        <mt-tab-container class="page-tabbar-container"
                          v-model="selected">

          <mt-tab-container-item id="病史">
            <div style=" overflow:auto;height:350px;">
              <div style="margin:10px;">
                <div>
                  <div class="text-title"> 主诉</div>
                  <div v-if="zhusu.timevalue!=''">
                    <div class="text-time">{{zhusu.timevalue}} </div>
                    <div class="text-content">{{zhusu.content}} </div>
                  </div>
                  <div v-else
                       class="text-no">
                    暂无
                  </div>
                </div>
                <hr>
                <div>
                  <div class="text-title"> 现病史</div>
                  <div v-if="xianbingshi.timevalue!=''">
                    <div class="text-time">{{xianbingshi.timevalue}} </div>
                    <div class="text-content">{{xianbingshi.content}} </div>

                  </div>
                  <div v-else
                       class="text-no">
                    暂无
                  </div>
                </div>
                <hr>
                <div>

                  <div class="text-title"> 过敏史</div>

                  <div v-if="guominshi.timevalue!=''">
                    <div class="text-time">{{guominshi.timevalue}} </div>
                    <div class="text-content">{{guominshi.content}} </div>
                  </div>
                  <div v-else
                       class="text-no">
                    暂无
                  </div>
                </div>
                <hr>
                <div>
                  <div class="text-title"> 疾病史</div>

                  <div v-if="jibingshi.timevalue!=''">
                    <div class="text-time">{{jibingshi.timevalue}} </div>
                    <div class="text-content">{{jibingshi.content}} </div>
                  </div>
                  <div v-else
                       class="text-no">
                    暂无
                  </div>

                </div>
                <hr>
                <div>
                  <div class="text-title"> 目前用药</div>
                  <div v-if="curdrugused.timevalue!=''">
                    <div class="text-time">{{curdrugused.timevalue}} </div>
                    <div class="text-content">{{curdrugused.content}} </div>
                  </div>
                  <div v-else
                       class="text-no">
                    暂无
                  </div>
                </div>
                <hr>

                <div v-show="zhusuphotos.length>0">
                  <div class="text-title">主诉图片</div>
                  <div class="text-time">
                    {{zhusuphoto.time}}</div>

                  <div v-viewer>
                    <div v-for="(item,index) in zhusuphotos"
                         :key="index">
                      <div align="center">
                        <img :src="item"
                             style="width: 50%; height: 50%; "
                             v-show="index==0">
                      </div>
                    </div>
                  </div>
                  <hr>

                </div>
              </div>

            </div>

          </mt-tab-container-item>

          <mt-tab-container-item id="体征">
            <div style="overflow:auto;height:350px;">
              <div v-if="dataTZ.length>0">
                <div v-for="(item,index) in dataTZ"
                     style="margin:10px; ">

                  <div class="text-title">{{item.OperationName}}</div>
                  <div class="text-time">{{item.OperationTime}}&nbsp;&nbsp;&nbsp;{{item.Address==1?'地点：会场':'地点：车辆'}}</div>

                  <div class="text-content">{{item.Detail}}</div>
                  <hr>
                </div>
              </div>
              <div v-else
                   style="margin:50px;text-align:center;font-size:20px;">

                暂无体征
              </div>
            </div>

          </mt-tab-container-item>

          <mt-tab-container-item id="处置">
            <div style="overflow:auto;height:350px;">
              <div v-if="dataCZ.length>0">
                <div v-for="(item,index) in dataCZ"
                     style="margin:10px; ">

                  <div class="text-title">{{item.OperationName}}&nbsp;&nbsp;&nbsp;<span v-show="(item.OperationCode=='P112')"
                          class="text-content">检查单号{{item.Detail1}}</span></div>
                  <div class="text-time">
                    {{item.OperationTime}}&nbsp;&nbsp;&nbsp;
                    {{item.Address==1?'地点：会场':'地点：车辆'}}</div>

                  <div class="text-content"><span>{{item.Detail}}</span></div>
                  <div align="center"
                       v-show="item.InfoType==3">
                    <div v-viewer>
                      <img :src="item.FileUrl"
                           style="width: 50%; height: 50%; ">
                    </div>
                  </div>
                  <hr>
                </div>
              </div>
              <div v-else
                   style="margin:50px;text-align:center;font-size:20px;">
                暂无处置
              </div>
            </div>

          </mt-tab-container-item>
        </mt-tab-container>

      </div>
      <el-divider></el-divider>

      <span class="text-title">专家列表</span>
      <div class="ass">

        <el-table :data="expertlist"
                  max-height="250"
                  :header-row-style="{'height':'14px','padding':'2px'}"
                  :header-cell-style="{'background-color': '#0070a8','color': 'white', 'text-align':'center','font-size':'14px'}">
          <el-table-column prop="PersonnelID"
                           v-if="false"></el-table-column>
          <el-table-column prop="Name"
                           label="姓名">

          </el-table-column>
          <el-table-column prop="DepartmentCode"
                           label="所在医院">

          </el-table-column>
          <el-table-column prop="TitleCode"
                           label="科室">

          </el-table-column>
          <el-table-column prop="Specialty"
                           label="专长">

          </el-table-column>
          <el-table-column label="操作">

            <template slot-scope="scope">

              <el-button type="primary"
                         size="mini"
                         @click="ptoe(scope.$index)">推送</el-button>

            </template>
          </el-table-column>

        </el-table>
      </div>
    </el-dialog>

  </div>
</template>
<script>
// import AMap from 'AMap';
import echarts from 'echarts';
import axios from 'axios';
import Gd_map from './Gd_map.vue';
import chart from './chart.vue'

import Car from './outline/Car.vue'
import Ass from './outline/Ass.vue'
import Hos from './outline/Hos.vue'
import overall from './outline/overall.vue'
import global from '../components/global.vue'

export default {

  components: {
    'Gd_map': Gd_map,
    'chart': chart,

    'Hos': Hos,
    'Car': Car,
    'Ass': Ass,
    'overall': overall,
  },
  provide() { // 注册一个方法
    return {
      reload: this.reload
    }
  },
  sockets: {
    news: function (data) {
      this.getmsg()
      this.getReportedPatientList()
      console.log('news信息收到')
    },
    news1: function (data) {
      this.getmsg()
      this.getReportedPatientList()
      console.log('news1信息收到')
    },
    alert: function (data) {
      this.getReportedPatientList()
      console.log('上报病人列表闪烁', data.AssemblyGroup)
      this.card = "second"
      this.shake($("#myexample"), "red", 4)
      // this.initialvalue()
      this.getPatientInfo(data.AssemblyGroup)
      this.getPatientRecord(data.AssemblyGroup)
      this.showPatinfo = true
      this.$nextTick(() => {
        this.getmsg()
      })
    },
    help: function (data) {
      this.getmsg()
      this.getReportedPatientList()
      console.log('help信息收到')
    },
  },
  data() {
    return {
      isRouterAlive: true,
      msg1: '世界互联网大会',
      msg2: '2019/3/12 3:27:17',
      msg3: '',
      msg4: '',
      mapd: Gd_map,
      // mapd:map,
      chartVue: '',
      title1: '消息列表',
      eid: '',
      title2: '总体情况',
      ReportedPatientList: [],

      loading: false,
      tablemsg: [],


      overallVisiable: true,
      showtype: 'overall',
      //小组类型
      grouptype: '',
      carlist: [],
      hoslist: [],
      asslist: [],
      expertlist: [],
      openmsg: '',
      showPatinfo: false,
      perinfo: {},
      memberId: '',
      toolipVisible: false,

      photosrc: global.photoUrl + "zyh_1557216080825test.jpg",
      intervalid1: null,
      watchID1: null,
      card: 'first',
      PatientId: '',
      selected: '病史',

      content: '',

      //过敏史
      guominshi: {
        content: '',
        timevalue: ''
      },
      //疾病史
      jibingshi: {
        content: '',
        timevalue: ''      },
      curdrugused: { timevalue: '', content: '', },
      zhusu: {        content: '',
        timevalue: ''

      }, xianbingshi: {
        content: '',
        timevalue: ''
      },
      // timevalue42: '',
      // timevalue43: '',
      timevalue2: '',
      // 主诉: '',
      // 主诉图片: [],
      zhusuphoto: {

        time: '',
      },
      // 主诉图片是数组,
      zhusuphotos: [],
      体征: '请选择体征',
      // 现病史: '',
      现病史图片: [],
      // 过敏史: '',
      // 疾病史: '',
      // 目前用药: '',
      既往史图片: [],
      初步诊断: '',


      dataTZ: [],
      dataCZ: [],
      patientrecord: [],


    }

  },

  mounted() {
    this.getTime();
    this.loadmap();  //加载地图和相关组件
    // this.$nextTick(() => {
    //   this.drawLine();    })
    // this.drawLine();

    this.$nextTick(() => {
      this.getmsg();


    })
    this.getcarlist();
    this.gethoslist();
    this.getasslist();
    this.getexpertlist();
    this.getReportedPatientList();





  },
  methods: {
    reload() {
      this.isRouterAlive = false
      this.$nextTick(function () {
        this.isRouterAlive = true
        //console.log('reload')
      })
    },

    getTime() {
      setInterval(() => {
        //new Date() new一个data对象，当前日期和时间
        // //toLocaleString() 方法可根据本地时间把 Date 对象转换为字符串，并返回结果。
        // this.msg3 = new Date(parseInt(time) * 1000).toLocaleString('chinese', { hour12: false }).replace(/:d{1,2}$/, ' ');



        this.msg3 = new Date().toLocaleString('chinese', { hour12: false });
        // this.msg4 = this.caltime(s1, this.msg2)
        var a = new Date(this.msg3.replace(/-/g, "/"));
        var b = new Date(this.msg2.replace(/-/g, "/"));
        this.caltime(a, b)
      }, 1000)
    },
    caltime(a, b) {
      var date3 = a - b;   //时间差的毫秒数     
      //计算出相差天数
      var days = Math.floor(date3 / (24 * 3600 * 1000))
      //计算出小时数
      var leave1 = date3 % (24 * 3600 * 1000)    //计算天数后剩余的毫秒数
      var hours = Math.floor(leave1 / (3600 * 1000))
      //计算相差分钟数
      var leave2 = leave1 % (3600 * 1000)        //计算小时数后剩余的毫秒数
      var minutes = Math.floor(leave2 / (60 * 1000))
      //计算相差秒数
      var leave3 = leave2 % (60 * 1000)      //计算分钟数后剩余的毫秒数
      var seconds = Math.round(leave3 / 1000)
      this.msg4 = days + " 天 " + hours + " 小时 " + minutes + " 分钟 " + seconds + " 秒"
    },
    changecomVisible(val) {
      console.log(val)

      this.showtype = 'overall'
      // this.overallVisiable = val
    },
    getAllStatusCount() {
      axios.get('/getAllStatusCount', {}).then((response) => {
        // console.log(response.data.results)
        var nn = response.data.results
        this.num1 = nn.chuzhiwancheng
        this.num2 = nn.chuzhizhong
        this.num3 = nn.daihousong
        this.num4 = nn.housongzhong
        this.num5 = nn.yichuyuan
        this.num6 = nn.yiruyuan
      }).catch(function (error) {
        console.log("error", error);
      })
    },
    loadmap() {
      const map = new AMap.Map('amap1', {
        zoom: 9
      });
    },

    //获取消息详情
    getmsg() {
      axios.get('/getAllMessage', {}).then((response) => {
        // console.log(response.data.results)
        //获取消息一次
        var cashdata = response.data.results
        var data1 = []
        for (var i = 0; i < cashdata.length; i++) {
          var a = cashdata[i].MessageTitle
          var b = cashdata[i].MessageDetail
          var c = b.split(/[\r\n]/)  //去除分隔号 
          var d = c.filter(i => i) //去除为空的项
          data1.push({ 'title': a, 'detail': d })
        }
        this.tablemsg = data1
      }).catch(function (error) {
        console.log("error", error);
      })
    },
    getChange(val) {
      //this.overallVisiable = false
      console.log(val)
      this.openmsg = val
      // console.log(this.openmsg)
      this.showtype = 'component'
      if (val.A) {

        this.chartVue = Ass
        this.eid = val.A

      }
      //测试Hos
      else if (val.H) {

        this.chartVue = Hos
        this.eid = val.H
      }
      else {

        this.chartVue = Car
        this.eid = val.C
      }

    },
    generate() {
      this.loading = true

      setTimeout(() => {
        this.loading = false;
        this.$refs.myChild.generateRecommend()
      }, 2000);


    },
    cancel() {
      this.$refs.myChild.cancelFlash()
    },
    openass() {
      this.showtype = 'grouplist'
      this.grouptype = 'ass'
    },
    chooseass(LocationNo) {

      this.getChange({ 'A': LocationNo })


    },
    opencar() {
      this.showtype = 'grouplist'
      this.grouptype = 'car'
    },
    choosecar(CarNo) {

      this.getChange({ 'C': CarNo })


    },

    openhos() {
      this.showtype = 'grouplist'
      this.grouptype = 'hos'
    },
    choosehos(OrganizationCode) {

      this.getChange({ 'H': OrganizationCode })


    },


    specialist() {
      this.showtype = 'grouplist'
      this.grouptype = 'expert'
    },
    getcarlist() {
      axios.get('/getCarList', {})
        .then((response) => {

          var tempdata = response.data.results
          this.carlist = tempdata.filter(item => {
            if (item.Longitude != null && item.Latitude != null) {

              if (item.CarStatus == 0) {
                item.CarStatus = '空闲'
              }
              else {
                item.CarStatus = '忙碌'

              }

              return item
            }

          })


          console.log(this.carlist)






        }).catch(function (error) {
          console.log("error", error);
        })


    },
    gethoslist() {
      axios.get('/getHosList', {})
        .then((response) => {
          this.hoslist = response.data.results
          console.log(this.hoslist, 'hos')
        }).catch(function (error) {
          console.log("error", error);
        })
    },
    getasslist() {
      axios.get('/getAssemblyList', {})
        .then((response) => {
          this.asslist = response.data.results

        }).catch(function (error) {
          console.log("error", error);
        })
    },
    getexpertlist() {
      axios.get('/getExpertList', {})
        .then((response) => {
          this.expertlist = response.data.results
          console.log(this.expertlist, 'expertlist')
        }).catch(function (error) {
          console.log("error", error);
        })

    },
    sendcall(val) {
      axios.post('/pushVideoLeader', {
        "GN": val
      }
      )
        .then((response) => {
          this.$message('已发送邀请')

        }).catch(function (error) {
          console.log("error", error);
        })

    },
    rowClick(row, column, event) {
      console.log(row.PatientId, '获取一次病历')
      //恢复病历初始值

      this.getPatientInfo(row.PatientId)
      this.getPatientRecord(row.PatientId)
      this.showPatinfo = true
    },
    initialvalue() {


      this.PatientId = ''
      this.selected = '病史'
      this.content = ''
      this.guominshi = { content: '', timevalue: '' }
      this.jibingshi = { content: '', timevalue: '' }
      this.curdrugused = { timevalue: '', content: '', }
      this.zhusu = { content: '', timevalue: '' }
      this.xianbingshi = {
        content: '',
        timevalue: ''
      }

      this.timevalue2 = ''

      this.zhusuphoto = {

        time: ''
      },

        this.zhusuphotos = []
      this.体征 = '请选择体征'

      this.现病史图片 = []

      this.既往史图片 = []
      this.初步诊断 = ''



      this.dataTZ = []
      this.dataCZ = []
      this.patientrecord = []


    },
    getReportedPatientList() {
      console.log('发起一次获取列表请求')
      axios.get('/getReportedPatientList', {}).then((response) => {

        //获取消息一次

        var nn = response.data.results
        const MapTable = new Map([[1, '男'], [2, '女'], [0, '不明']])
        this.ReportedPatientList = nn.map(item => {
          item.Gender = MapTable.get(item.Gender)
          return item


        })


      }).catch(function (error) {
        console.log("error", error);
      })


    },
    getPatientInfo(a) {
      axios.post('/getPatientInfo', {
        "patientId": a
      }
      )
        .then((response) => {
          // console.log(response.data.results[0])
          const MapTable = new Map([[1, '男'], [2, '女'], [0, '不明']])
          var dd = response.data.results[0]
          dd.Gender = MapTable.get(dd.Gender)
          this.perinfo = dd
          // console.log(this.perinfo)


        }).catch(function (error) {
          console.log("error", error);
        })


    },
    getPatientRecord(ss) {
      //每次获取病历前先清空病历初始值
      this.initialvalue()
      axios.post('/getPatientRecord', {
        patientId: ss
      }).then((response) => {

        this.patientrecord = response.data.results
        //主诉
        // this.zhusu = this.patientrecord.P01;
        if (this.patientrecord.P01.length > 0) {
          this.zhusuphotos = []
          for (var i = 0; i < this.patientrecord.P01.length; i++) {
            if (this.patientrecord.P01[i].InfoType == 3) {
              var fileUrl = this.patientrecord.P01[i].FileUrl
              var time = this.patientrecord.P01[i].OperationTime
              // this.zhusuphoto.fileUrl = global.photoUrl + fileUrl
              this.zhusuphoto.time = time
              // this.zhusuphoto.unshift({
              //   fileUrl: global.photoUrl + fileUrl,
              //   time: time
              // })

              this.zhusuphotos.push(global.photoUrl + fileUrl)
            }
          }


          for (var k = 0; k < this.patientrecord.P01.length; k++) {
            if (this.patientrecord.P01[k].InfoType == 1) {
              this.zhusu.content = this.patientrecord.P01[k].Detail;
              this.zhusu.timevalue = this.patientrecord.P01[k].OperationTime;
              break;
            }
          }
        } else {
          this.zhusu.content = '暂无';
          this.zhusu.timevalue = '';
        }
        //现病史
        if (this.patientrecord.P02.length > 0) {
          for (var i = 0; i < this.patientrecord.P02.length; i++) {
            if (this.patientrecord.P02[i].InfoType == 3) {
              var fileUrl = this.patientrecord.P02[i].FileUrl
              var time = this.patientrecord.P02[i].OperationTime
              this.现病史图片.unshift({
                fileUrl: global.photoUrl + fileUrl,
                time: time
              })
            }
          }
          console.log(this.现病史图片)
          for (var k = 0; k < this.patientrecord.P02.length; k++) {
            if (this.patientrecord.P02[k].InfoType == 1) {
              this.xianbingshi.content = this.patientrecord.P02[k].Detail;
              this.xianbingshi.timevalue = this.patientrecord.P02[k].OperationTime;
              break;
            }
          }
        } else {
          this.xianbingshi.content = '暂无';
          this.xianbingshi.timevalue = '';
        }
        //体征
        this.dataTZ = this.patientrecord.P03

        //既往史
        for (var i = 0; i < this.patientrecord.P04.length; i++) {
          if (this.patientrecord.P04[i].OperationCode == "P041") {
            this.guominshi.content = this.patientrecord.P04[i].Detail
            this.guominshi.timevalue = this.patientrecord.P04[i].OperationTime;

            break;
          }
        }
        for (var k = 0; k < this.patientrecord.P04.length; k++) {
          if (this.patientrecord.P04[k].OperationCode == "P042") {
            this.jibingshi.content = this.patientrecord.P04[k].Detail
            this.jibingshi.timevalue = this.patientrecord.P04[k].OperationTime;
            break;
          }
        }
        // console.log(this.patientrecord.P04[k], 'P04指标')
        for (var j = 0; j < this.patientrecord.P04.length; j++) {
          if (this.patientrecord.P04[j].OperationCode == "P043") {
            this.curdrugused.content = this.patientrecord.P04[j].Detail
            this.curdrugused.timevalue = this.patientrecord.P04[j].OperationTime;
            break;
          }
        }
        for (var j = 0; j < this.patientrecord.P04.length; j++) {
          if (this.patientrecord.P04[j].OperationCode == "P040") {
            var fileUrl = this.patientrecord.P04[j].FileUrl
            var time = this.patientrecord.P04[j].OperationTime
            this.既往史图片.unshift({
              fileUrl: global.photoUrl + fileUrl,
              time: time
            })
          }
        }
        //初步诊断
        if (this.patientrecord.P05.length > 0) {
          this.初步诊断 = this.patientrecord.P05[0].Detail
          this.timevalue2 = this.patientrecord.P05[0].OperationTime
        } else {
          this.初步诊断 = ''
          this.timevalue2 = ''
        }
        //处置方案
        this.dataCZ = this.patientrecord.P11
        for (var j = 0; j < this.patientrecord.P11.length; j++) {
          if (this.patientrecord.P11[j].InfoType == 3) {
            var fileUrl = this.patientrecord.P11[j].FileUrl
            this.dataCZ[j].FileUrl = global.photoUrl + fileUrl

          }
        }

        // console.log(this.dataCZ, '既往处置')
        //医嘱
        if (this.patientrecord.P06.length > 0) {
          this.doctortell = this.patientrecord.P06[0].Detail
        } else {
          this.doctortell = ''
        }
      })



    },
    getMemberInfo(memberId) {
      //确定 取消是否要查看真名
      this.toolipVisible = false




      axios.post('/getMemberInfo', {
        "memberId": memberId
      }
      )
        .then((response) => {

          var cc = response.data.results[0]

          this.perinfo.Name = cc.Name

        }).catch(function (error) {
          console.log("error", error);
        })
    },
    refresh(PatientId) {
      this.getPatientInfo(PatientId)


      this.getPatientRecord(PatientId)
    },
    click1() {
      this.card = "first"
      console.log('消息列表')
    },
    click2() {
      this.card = "second"
      console.log('上报病人列表')
    },
    shake(ele, cls, times) {//边框闪烁   
      //ele要闪动的元素 cls闪动的类 times 闪动几次  
      var i = 0, t = false, o = ele.attr("class") + " ", c = "", times = times || 2;
      if (t) return;
      t = setInterval(function () {        i++; c = i % 2 ? o + cls : o; ele.attr("class", c);
        if (i == 2 * times) { clearInterval(t); ele.removeClass(cls); }      }, 200);
    },
    handleClose(done) {
      done();
    },
    ptoe(index) {
      axios.post('/newExpertPatient', {
        "patientId": this.perinfo.PatientId,
        "expertId": this.expertlist[index].PersonnelID
      })
        .then(response => {
          // this.$message('推送成功')
          console.log(response.data)

        })
        .catch(function (error) {
          console.log("error", error);
        });

    },





  }}
</script>
<style>
.groupbox-boarder {
  border: 1px solid #ccc;
  box-shadow: 0px 0px 0px 0px #000;
}
.one-of-groupbox-boarder {
  padding: 0 10px;
  text-align: left;
  font-size: 14px;
}
.demo-table .el-table th,
.demo-table .el-table tr {
  background-color: #c6e2ff;
}
.demo-table .el-table,
.demo-table .el-table__expanded-cell {
  background-color: #d9ecff;
}
.demo-table .el-table--enable-row-hover .el-table__body tr:hover > td {
  background-color: #a0cfff;
}
.text-group {
  padding: 5px;
  /* color: white; */
}
.picture_bodyy {
  padding: 10px;
  border: 1px solid #808288;

  text-align: left;
  color: whitesmoke;
  max-height: 510px;
  overflow: auto;
}
.picture_body22 {
  border: 1px solid #808288;
  text-align: left;
  color: whitesmoke;
  height: 170px;
}
.red {
  border: 4px solid #d00;
}
.text-title {
  font-size: 18px;
  font-weight: bold;
}
.text-time {
  font-size: 14px Small;
  color: gray;
  padding: 10px;
}
.text-content {
  font-size: 16px;
  margin: 0px 10px;
}

.text-no {
  font-size: 16px;

  margin: 10px;
}
.card-title {
  font-size: 18px;
  font-weight: bold;
  color: #99c1ae;
}
.demo-table1 .el-table th,
.demo-table1 .el-table tr {
  background-color: #c6e2ff;
}
.demo-table1 .el-table,
.demo-table1 .el-table__expanded-cell {
  background-color: #adcbe9;
}
.demo-table1 .el-table--enable-row-hover .el-table__body tr:hover > td {
  background-color: #a0cfff;
}
</style>


