<template>
  <div>

    <img src="../assets/header.png">

    <div class="css1">
      <el-tabs tab-position="left"
               type="border-card"
               v-model="activeTabName"
               @tab-click="changetab">

        <el-tab-pane label="首页"
                     name="A">
          <componet is='test'>
          </componet>
        </el-tab-pane>

        <el-tab-pane label="医疗业务"
                     name="B">

          <componet is='medicalb'
                    v-if="isshow2"> </componet>

        </el-tab-pane>
        <el-tab-pane label="医疗资源"
                     name="C">
          <componet is='medicalresource'
                    v-if="isshow3">
          </componet>
        </el-tab-pane>

        <el-tab-pane label="消息平台"
                     name="D">
          <componet is='messagelist'
                    v-if="isshow4">
          </componet>
        </el-tab-pane>
        <el-tab-pane label="后台管理"
                     name="E">
          <componet is='backgroundmanagement'
                    v-if="isshow5">
          </componet>
        </el-tab-pane>
      </el-tabs>
    </div>

  </div>
</template>
<script>
import axios from 'axios';
import test from './test.vue'

import medicalb from './medicalb.vue'
import medicalresource from './medicalresource.vue'
import messagelist from './messagelist.vue'
import backgroundmanagement from './backgroundmanagement.vue'
export default {
  components: {
    'test': test,

    'medicalb': medicalb,
    'medicalresource': medicalresource,
    'messagelist': messagelist,
    'backgroundmanagement': backgroundmanagement
  },
  provide() { // 注册一个方法
    return {
      reload: this.reload
    }
  },
  sockets: {

    news: function (data) {
      //收到的消息会在这里
      console.log('首页 收到普通消息')

      this.$notify.info({
        title: '通知',
        message: data.MessageTitle,
        duration: 300000
        // duration: 0
      });
    },
    news1: function (data) {
      console.log('我知道了2');
      this.$notify.info({
        title: '通知',
        message: data.MessageTitle,
        duration: 300000

      });
    },
    alert: function (data) {
      //收到的消息会在这里
      console.log('首页 收到病人求助消息')

      this.$notify({
        title: '上报病人',
        message: data.MessageTitle,
        type: 'warning',
        duration: 300000
        // duration: 0
      });
    },
    alert0: function (data) {
      this.$notify({
        title: '上报病人',
        message: data.MessageTitle,
        type: 'warning',
        duration: 300000
        // duration: 0
      });
    },
    help: function (data) {
      //收到的消息会在这里
      console.log('首页 收到普通求助消息')

      this.$notify({
        title: '请求远程支持',
        message: data.MessageTitle,
        type: 'warning',
        duration: 20000
      });
    },

  },
  data() {
    return {
      msg1: '互联网大会',
      msg2: '2019/9/25 16:00:00',
      isRouterAlive: true,
      msg3: '',
      msg4: '',
      num1: '',
      num2: '',
      num3: '',
      num4: '',
      num5: '',
      num6: '',
      activeTabName: 'A',
      isshow2: false,
      isshow3: false,
      isshow4: false,
      isshow5: false


    }
  },
  mounted() {
    this.getTime();

    this.getAllStatusCount();
  },
  methods: {
    reload() {
      this.isRouterAlive = false
      this.$nextTick(function () {
        this.isRouterAlive = true
        //console.log('reload')
      })
    },
    changetab(tab, b) {
      console.log('切换tab')
      switch (tab.name) {
        case 'B':
          this.isshow3 = false
          this.isshow4 = false
          this.isshow5 = false
          this.isshow2 = true
          break
        case 'C':
          this.isshow2 = false
          this.isshow3 = true
          this.isshow4 = false
          this.isshow5 = false
          break
        case 'D':
          this.isshow2 = false
          this.isshow4 = true
          this.isshow3 = false
          this.isshow5 = false
          break
        case 'E':
          this.isshow2 = false
          this.isshow5 = true
          this.isshow4 = false
          this.isshow3 = false
          break
        default:
          break
      }
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

    getAllStatusCount() {
      axios.get('/getAllStatusCount', {}).then((response) => {
        // console.log(response.data.results)
        var nn = response.data.results
        this.num1 = nn.chuzhiwancheng
        this.num2 = nn.chuzhizhong
        this.num3 = nn.daihousong
        this.num4 = nn.housongzhong
        this.num6 = nn.yichuyuan
        this.num5 = nn.yiruyuan



      }).catch(function (error) {
        console.log("error", error);
      })
    },

  }
}

</script>
<style>
.css1 .el-tabs--border-card {
  background: #74adda;
}
.css1 .el-tabs--border-card > .el-tabs__header {
  background-color: #4575b4;
}
/* tabs悬浮颜色 */
.css1 .el-tabs--border-card > .el-tabs__header .el-tabs__item.is-active {
  background-color: #004881;
}
/* tabs名字改成白色 */
.css1 .el-tabs--border-card > .el-tabs__header .el-tabs__item {
  color: #f2f6fc;
  font-size: 16px;
  text-align: center;
}
</style>
