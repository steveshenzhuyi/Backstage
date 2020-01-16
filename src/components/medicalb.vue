<template>
  <div style="height:894px;">
    <el-row>

      <el-date-picker v-model="value1"
                      type="date"
                      :editable="false"
                      :clearable="false"
                      placeholder="选择日期"
                      :picker-options="pickerOptions"
                      @change="choosedate1()"
                      format="yyyy 年 MM 月 dd 日"
                      value-format="yyyy-MM-dd">
      </el-date-picker>
      &nbsp; &nbsp;
      <el-button @click="reset()">重置</el-button>
    </el-row>
    <el-row :gutter="20"
            style="padding:10px">
      <el-col :span="8">

        <div ref="chart1"
             style="height:400px;"></div>

      </el-col>
      <el-col :span="8">

        <div ref="chart2"
             style="height:400px;"></div>

      </el-col>
      <el-col :span="8">

        <div ref="chart3"
             style="height:400px;"></div>

      </el-col>
    </el-row>
    <el-row :gutter="20"
            style="padding:10px">
      <el-col :span="12">

        <div ref="barchart1"
             style="height:400px;"></div>

      </el-col>
      <el-col :span="12">

        <div ref="barchart2"
             style="height:400px;"></div>

      </el-col>
    </el-row>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  inject: ['reload'],
  data() {
    return {
      value1: '',
      pickerOptions: {
        disabledDate(time) {
          return time.getTime() > Date.now();
        }
      }

    }
  },
  mounted() {
    console.log('进入医疗业务页面')
    this.$nextTick(() => {
      this.drawpie1();
      this.drawpie2();
      this.drawpie3();
      this.drawbar1()
      this.drawbar2()
    })

  },
  methods: {
    choosedate1() {
      console.log(this.value1)
      this.Redrawpie1(this.value1);
      this.Redrawpie2(this.value1);
      this.Redrawpie3(this.value1);
      this.Redrawbar1(this.value1)
      this.Redrawbar2(this.value1)
    },
    confirm() {

    },
    reset() {
      console.log('重置')
      this.value1 = ''
      this.drawpie1();
      this.drawpie2();
      this.drawpie3();
      this.drawbar1()
      this.drawbar2()
    },

    drawpie1() {
      let chart = this.$echarts.init(this.$refs.chart1, 'dark')
      // console.log(response.data.results)
      axios.get('/getAllStatusCount', {}).then((response) => {
        var data1 = [];
        var a = response.data.results.chuzhizhong;
        var b = response.data.results.chuzhiwancheng;
        var c = response.data.results.daihousong;
        var d = response.data.results.housongzhong;
        var e = response.data.results.yiruyuan;
        var f = response.data.results.yichuyuan;

        if (a > 0) {
          data1.push({ name: '处置中', value: a })
        }
        if (b > 0) {
          data1.push({ name: '处置完成', value: b })
        }
        if (c > 0) {
          data1.push({ name: '待后送', value: c })
        }
        if (d > 0) {
          data1.push({ name: '后送中', value: d })
        }
        if (e > 0) {
          data1.push({ name: '已入院', value: e })
        }
        if (f > 0) {
          data1.push({ name: '已出院', value: f })
        }
        if (data1.length < 1) {
          data1.push({ name: '处置中', value: a }, { name: '处置完成', value: b }, { name: '待后送', value: c }, { name: '后送中', value: d }, { name: '已入院', value: e }, { name: '已出院', value: f })
        }

        //data1.push({ name: '处置中', value: a }, { name: '处置完成', value: b }, { name: '待后送', value: c }, { name: '待后送', value: c },{ name: '待后送', value: c }, { name: '已出院', value: f })
        chart.setOption({
          title: {
            text: '病人状态统计'

          },
          tooltip: {
            trigger: 'item',
            formatter: "{a} <br/>{b}: {c} ({d}%)"
          },

          series: [
            {
              name: '病人状态统计',
              type: 'pie',
              radius: ['30%', '60%'],
              center: ['55%', '50%'],
              avoidLabelOverlap: false,
              data: data1
            }
          ]
        })
      }).catch(function (error) {
        console.log("error", error);
      })
    },
    Redrawpie1(val) {
      let chart = this.$echarts.init(this.$refs.chart1, 'dark')
      // console.log(response.data.results)
      axios.post('/getAllStatusCountbyDate', {
        "time": val
      }).then((response) => {
        console.log('根据日期获取一次数据')

        var data1 = [];
        var a = response.data.results.chuzhizhong;
        var b = response.data.results.chuzhiwancheng;
        var c = response.data.results.daihousong;
        var d = response.data.results.housongzhong;
        var e = response.data.results.yiruyuan;
        var f = response.data.results.yichuyuan;

        if (a > 0) {
          data1.push({ name: '处置中', value: a })
        }
        if (b > 0) {
          data1.push({ name: '处置完成', value: b })
        }
        if (c > 0) {
          data1.push({ name: '待后送', value: c })
        }
        if (d > 0) {
          data1.push({ name: '后送中', value: d })
        }
        if (e > 0) {
          data1.push({ name: '已入院', value: e })
        }
        if (f > 0) {
          data1.push({ name: '已出院', value: f })
        }
        if (data1.length < 1) {
          data1.push({ name: '处置中', value: 0 }, { name: '处置完成', value: 0 }, { name: '待后送', value: 0 }, { name: '后送中', value: 0 }, { name: '已入院', value: 0 }, { name: '已出院', value: 0 })
        }



        chart.setOption({
          title: {
            text: '病人状态统计'
          },
          tooltip: {
            trigger: 'item',
            formatter: "{a} <br/>{b}: {c} ({d}%)"
          },

          series: [
            {
              name: '病人状态统计',
              type: 'pie',
              radius: ['30%', '60%'],
              center: ['55%', '50%'],
              avoidLabelOverlap: false,
              data: data1
            }
          ]
        })
      }).catch(function (error) {
        console.log("error", error);
      })
    },

    drawpie2() {
      let chart = this.$echarts.init(this.$refs.chart2, 'dark')
      axios.get('/getAllClassificationCount', {})
        .then((response) => {
          var cdata = response.data.results
          console.log(cdata, 'pie2')

          var f = 0
          var a = 0
          var b = 0
          var c = 0
          var d = 0
          var e = 0
          var data1 = [];
          cdata.forEach(item => {
            if (item.Classification == 0) {
              f = cdata.find(item => item.Classification == 0).count

              data1.push({ name: '未分级', value: f })
            }
            else {
              f = 0
            }
            if (item.Classification == 1) {
              a = cdata.find(item => item.Classification == 1).count

              data1.push({ name: 'Ⅰ级', value: a })            }
            else {
              a = 0
            }
            if (item.Classification == 2) {
              b = cdata.find(item => item.Classification == 2).count

              data1.push({ name: 'Ⅱ级', value: b })            }
            else {
              b = 0
            }
            if (item.Classification == 3) {
              c = cdata.find(item => item.Classification == 3).count

              data1.push({ name: 'Ⅲ级', value: c })            }
            else {
              c = 0
            }
            if (item.Classification == 4) {
              d = cdata.find(item => item.Classification == 4).count

              data1.push({ name: 'Ⅳ级', value: d })            }
            else {
              d = 0
            }
            if (item.Classification == 5) {
              e = cdata.find(item => item.Classification == 5).count

              data1.push({ name: 'Ⅴ级', value: e })            }
            else {
              e = 0
            }
          })
          if (data1.length < 1) {
            data1.push({ name: 'Ⅰ级', value: 0 }, { name: 'Ⅱ级', value: 0 }, { name: 'Ⅲ级', value: 0 }, { name: 'Ⅳ级', value: 0 }, { name: '未分级', value: 0 }, { name: 'Ⅴ级', value: 0 })
          }

          // var data1 = [];
          // data1.push({ name: 'Ⅰ级', value: a }, { name: 'Ⅱ级', value: b }, { name: 'Ⅲ级', value: c }, { name: 'Ⅳ级', value: d }, { name: '未分级', value: f })
          chart.setOption({
            title: {
              text: '病情级别统计'

            },
            tooltip: {
              trigger: 'item',
              formatter: "{a} <br/>{b}: {c} ({d}%)"
            },

            series: [
              {
                name: '病情级别统计',
                type: 'pie',
                radius: ['30%', '60%'],
                center: ['55%', '50%'],
                avoidLabelOverlap: false,
                data: data1
              }
            ]
          })
        })
        .catch(function (error) {
          console.log("error", error);
        })

    },
    Redrawpie2(val) {
      let chart = this.$echarts.init(this.$refs.chart2, 'dark')
      axios.post('/getAllClassificationCountbyDate', {
        time: val
      })
        .then((response) => {
          console.log('根据日期获取一次数据pie2')
          var cdata = response.data.results
          var f = 0
          var a = 0
          var b = 0
          var c = 0
          var d = 0
          var e = 0
          var data1 = [];
          cdata.forEach(item => {
            if (item.Classification == 0) {
              f = cdata.find(item => item.Classification == 0).count

              data1.push({ name: '未分级', value: f })
            }
            else {
              f = 0
            }
            if (item.Classification == 1) {
              a = cdata.find(item => item.Classification == 1).count

              data1.push({ name: 'Ⅰ级', value: a })            }
            else {
              a = 0
            }
            if (item.Classification == 2) {
              b = cdata.find(item => item.Classification == 2).count

              data1.push({ name: 'Ⅱ级', value: b })            }
            else {
              b = 0
            }
            if (item.Classification == 3) {
              c = cdata.find(item => item.Classification == 3).count

              data1.push({ name: 'Ⅲ级', value: c })            }
            else {
              c = 0
            }
            if (item.Classification == 4) {
              d = cdata.find(item => item.Classification == 4).count
              data1.push({ name: 'Ⅳ级', value: d })            }
            else {
              d = 0
            }
            if (item.Classification == 5) {
              e = cdata.find(item => item.Classification == 5).count

              data1.push({ name: 'Ⅴ级', value: e })            }
            else {
              e = 0
            }
          })
          if (data1.length < 1) {
            data1.push({ name: 'Ⅰ级', value: 0 }, { name: 'Ⅱ级', value: 0 }, { name: 'Ⅲ级', value: 0 }, { name: 'Ⅳ级', value: 0 }, { name: '未分级', value: 0 }, { name: 'Ⅴ级', value: 0 })
          }
          chart.setOption({
            title: {
              text: '病情级别统计'

            },
            tooltip: {
              trigger: 'item',
              formatter: "{a} <br/>{b}: {c} ({d}%)"
            },

            series: [
              {
                name: '病情级别统计',
                type: 'pie',
                radius: ['30%', '60%'],
                center: ['55%', '50%'],
                avoidLabelOverlap: false,
                data: data1
              }
            ]
          })
        })
        .catch(function (error) {
          console.log("error", error);
        })
    },
    drawpie3() {
      let chart = this.$echarts.init(this.$refs.chart3, 'dark')

      axios.get('/getAllOperationCount', {}).then((response) => {

        var cdata = response.data.results
        // console.log(cdata)
        var ddata = []
        cdata.forEach(item => {
          if (['P111', 'P112', 'P113', 'P114', 'P115', 'P116', 'P117'].includes(item.OperationCode)) {            ddata.push({ name: item.OperationName, value: item.count })
          }

        })

        chart.setOption({
          title: {
            text: '处置情况统计'

          },
          tooltip: {
            trigger: 'item',
            formatter: "{a} <br/>{b}: {c} ({d}%)"
          },

          series: [
            {
              name: '处置情况统计',
              type: 'pie',
              radius: ['30%', '60%'],
              center: ['55%', '50%'],
              avoidLabelOverlap: false,

              data: ddata
            }
          ]        })

      })
        .catch(function (error) {
          console.log("error", error);
        })
    },

    Redrawpie3(val) {
      let chart = this.$echarts.init(this.$refs.chart3, 'dark')

      axios.post('/getAllOperationCountbyDate', {
        "time": val
      }).then((response) => {

        var cdata = response.data.results
        // console.log(cdata)
        var ddata = []

        cdata.forEach(item => {
          if (['P111', 'P112', 'P113', 'P114', 'P115', 'P116', 'P117'].includes(item.OperationCode)) {
            ddata.push({ name: item.OperationName, value: item.count })
          }

        })
        if (ddata.length < 1) {
          ddata.push(
            { name: '吸氧', value: 0 },
            { name: "心电图", value: 0 },
            { name: "止血包扎", value: 0 },
            { name: "支具固定", value: 0 },
            { name: "口服药物", value: 0 },
            { name: "静脉给药", value: 0 },
            { name: "其它处理", value: 0 }
          )
        }

        chart.setOption({
          title: {
            text: '处置情况统计'

          },
          tooltip: {
            trigger: 'item',
            formatter: "{a} <br/>{b}: {c} ({d}%)"
          },

          series: [
            {
              name: '处置情况统计',
              type: 'pie',
              radius: ['30%', '60%'],
              center: ['55%', '50%'],
              avoidLabelOverlap: false,

              data: ddata
            }
          ]        })

      })
        .catch(function (error) {
          console.log("error", error);
        })
    },



    drawbar1() {
      let barchart = this.$echarts.init(this.$refs.barchart1, 'dark')
      axios.get('/getAssemblyList', {})
        .then((response) => {
          var tt = response.data.results
          var data1 = []
          var data2 = []
          var data3 = []
          var p1 = []
          for (var i = 0; i < tt.length; i++) {
            if (tt[i].GroupNo != null) {
              data1.push(tt[i].LocationName)

              p1.push(
                this.$axios.post('getEmergencyStatusCount', {
                  "groupNo": tt[i].GroupNo
                }))
            }
          }
          Promise.all(p1).then(res => {
            console.log(res);
            for (var j = 0; j < res.length; j++) {
              data2.push(res[j].data.results.chuzhizhong + res[j].data.results.chuzhiwancheng)
              data3.push(res[j].data.results.daihousong + res[j].data.results.yihousong)
            }
            console.log('会场处置统计')


            barchart.setOption({
              title: {
                text: '会场情况统计'

              },
              tooltip: {
                trigger: 'axis',
                axisPointer: {
                  type: 'shadow'
                }
              },
              legend: {
                data: ['现场处置人数', '后送人数']
              },
              grid: {
                left: '3%',
                right: '4%',
                bottom: '3%',
                containLabel: true
              },
              xAxis: {
                type: 'category',
                data: data1

              },
              yAxis: {

                type: 'value',


              },
              series: [
                {
                  name: '现场处置人数',
                  type: 'bar',
                  data: data2,
                  label: {
                    normal: {
                      show: true,
                      position: 'top'
                    }
                  },
                },
                {
                  name: '后送人数',
                  type: 'bar',
                  data: data3,
                  label: {
                    normal: {
                      show: true,
                      position: 'top'
                    }
                  },
                },
              ]
            })

          }).catch(
            (err) => {
              console.log(err)
            }
          )
        }).catch(function (error) {
          console.log("error", error);
        })

    },
    Redrawbar1(val) {
      let barchart = this.$echarts.init(this.$refs.barchart1, 'dark')
      axios.get('/getAssemblyList', {})
        .then((response) => {
          var tt = response.data.results
          var data1 = []
          var data2 = []
          var data3 = []
          var p1 = []
          for (var i = 0; i < tt.length; i++) {
            if (tt[i].GroupNo != null) {
              data1.push(tt[i].LocationName)
              // console.log(tt[i].GroupNo)
              p1.push(
                this.$axios.post('getEmergencyStatusCountbyDate', {
                  "groupNo": tt[i].GroupNo,
                  "time": val
                }))
            }
          }
          Promise.all(p1).then(res => {
            // console.log(res);
            for (var j = 0; j < res.length; j++) {
              data2.push(res[j].data.results.chuzhizhong + res[j].data.results.chuzhiwancheng)
              data3.push(res[j].data.results.daihousong + res[j].data.results.yihousong)
            }
            console.log('会场处置统计')


            barchart.setOption({
              title: {
                text: '会场情况统计'

              },
              tooltip: {
                trigger: 'axis',
                axisPointer: {
                  type: 'shadow'
                }
              },
              legend: {
                data: ['现场处置人数', '后送人数']
              },
              grid: {
                left: '3%',
                right: '4%',
                bottom: '3%',
                containLabel: true
              },
              xAxis: {
                type: 'category',
                data: data1

              },
              yAxis: {

                type: 'value',


              },
              series: [
                {
                  name: '现场处置人数',
                  type: 'bar',
                  data: data2,
                  label: {
                    normal: {
                      show: true,
                      position: 'top'
                    }
                  },
                },
                {
                  name: '后送人数',
                  type: 'bar',
                  data: data3,
                  label: {
                    normal: {
                      show: true,
                      position: 'top'
                    }
                  },
                },
              ]
            })

          }).catch(
            (err) => {
              console.log(err)
            }
          )
        }).catch(function (error) {
          console.log("error", error);
        })

    },

    drawbar2() {
      let barchart = this.$echarts.init(this.$refs.barchart2, 'dark')

      axios.get('/getHosList', {})
        .then((response) => {
          var tt = response.data.results
          var data1 = []
          var data2 = []
          var data3 = []
          var data4 = []


          var p1 = []
          for (var i = 0; i < tt.length; i++) {
            if (tt[i].GroupNo != null) {
              data1.push(tt[i].OrganizationName)
              // console.log(tt[i].GroupNo)
              p1.push(
                this.$axios.post('getHosStatusCount', {
                  "groupNo": tt[i].GroupNo
                }))
            }
          }


          Promise.all(p1).then(res => {
            // console.log(res);

            for (var j = 0; j < res.length; j++) {
              data2.push(res[j].data.results.daisongda)
              data3.push(res[j].data.results.yiruyuan)
              data4.push(res[j].data.results.yichuyuan)
            }

            // console.log('第二步')


            barchart.setOption({
              title: {
                text: '医院情况统计',

              },
              tooltip: {
                trigger: 'axis',
                axisPointer: {
                  type: 'shadow'
                }
              },
              legend: {
                data: ['待后送', '已入院', '已出院']
              },
              grid: {
                left: '3%',
                right: '4%',
                bottom: '3%',
                containLabel: true
              },
              xAxis: {
                type: 'category',
                data: data1

              },
              yAxis: {

                type: 'value',


              },
              series: [
                {
                  name: '待后送',
                  type: 'bar',
                  data: data2,
                  label: {
                    normal: {
                      show: true,
                      position: 'top'
                    }
                  },

                },
                {
                  name: '已入院',
                  type: 'bar',
                  data: data3,
                  label: {
                    normal: {
                      show: true,
                      position: 'top'
                    }
                  },
                },
                {
                  name: '已出院',
                  type: 'bar',
                  data: data4,
                  label: {
                    normal: {
                      show: true,
                      position: 'top'
                    }
                  },
                },
              ]
            })


          }).catch(
            (err) => {
              console.log(err)
            }
          )
        }).catch(function (error) {
          console.log("error", error);
        })
    },
    Redrawbar2(val) {
      let barchart = this.$echarts.init(this.$refs.barchart2, 'dark')

      axios.get('/getHosList', {})
        .then((response) => {
          var tt = response.data.results
          var data1 = []
          var data2 = []
          var data3 = []
          var data4 = []


          var p1 = []
          for (var i = 0; i < tt.length; i++) {
            if (tt[i].GroupNo != null) {
              data1.push(tt[i].OrganizationName)
              // console.log(tt[i].GroupNo)
              p1.push(
                this.$axios.post('getHosStatusCountbyDate', {
                  "groupNo": tt[i].GroupNo,
                  "time": val
                }))
            }
          }


          Promise.all(p1).then(res => {
            // console.log(res);

            for (var j = 0; j < res.length; j++) {
              data2.push(res[j].data.results.daisongda)
              data3.push(res[j].data.results.yiruyuan)
              data4.push(res[j].data.results.yichuyuan)
            }

            // console.log('第二步')


            barchart.setOption({
              title: {
                text: '医院情况统计',

              },
              tooltip: {
                trigger: 'axis',
                axisPointer: {
                  type: 'shadow'
                }
              },
              legend: {
                data: ['待后送', '已入院', '已出院']
              },
              grid: {
                left: '3%',
                right: '4%',
                bottom: '3%',
                containLabel: true
              },
              xAxis: {
                type: 'category',
                data: data1

              },
              yAxis: {

                type: 'value',


              },
              series: [
                {
                  name: '待后送',
                  type: 'bar',
                  data: data2,
                  label: {
                    normal: {
                      show: true,
                      position: 'top'
                    }
                  },

                },
                {
                  name: '已入院',
                  type: 'bar',
                  data: data3,
                  label: {
                    normal: {
                      show: true,
                      position: 'top'
                    }
                  },
                },
                {
                  name: '已出院',
                  type: 'bar',
                  data: data4,
                  label: {
                    normal: {
                      show: true,
                      position: 'top'
                    }
                  },
                },
              ]
            })


          }).catch(
            (err) => {
              console.log(err)
            }
          )
        }).catch(function (error) {
          console.log("error", error);
        })
    }


  }
}
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
.cardcss1 {
  background-color: #333333;
  color: white;
  font-family: "Helvetica Neue";
  font-size: 20px;
}
</style>


