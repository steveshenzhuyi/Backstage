<template>
  <div class="overallcss">
    <div class="picture_header">
      <div class="picture_title">
        总体情况
      </div>
    </div>
    <div class="picture_bodyo">

      <div ref="barchart1"
           style="height:250px; padding:10px"></div>

      <div ref="barchart2"
           style="height:250px;padding-left:10px;padding-right:10px;padding-bottom:10px;"></div>

    </div>
  </div>
</template>
<script>
import echarts from 'echarts';
import axios from 'axios';

export default {
  // inject: ['reload'],
  data() {
    return {

    }
  },
  sockets: {
    news: function (data) {

      this.drawbar1()
      this.drawbar2()
    },
    news1: function (data) {
      this.drawbar1()
      this.drawbar2()
    },
    alert: function (data) {
      this.drawbar1()
      this.drawbar2()
    },
    help: function (data) {
      this.drawbar1()
      this.drawbar2()
    },
  },

  mounted() {
    console.log('进入概览页面')
    this.$nextTick(() => {
      this.drawbar1();
      this.drawbar2();
    })
  },
  methods: {
    drawbar1() {
      console.log('画图1')
      let barchart = this.$echarts.init(this.$refs.barchart1, 'dark')
      axios.get('getAllStatusCount', {})
        .then((response) => {
          var tt = response.data.results
          // console.log(tt) 是一个对象


          var data1 = ['处置中', '处置完成', '待后送', '后送中', '已入院', '已出院']
          var data2 = []
          for (let key in tt) {
            data2.push(tt[key])
          }

          barchart.setOption({
            title: {
              text: '病人状态统计'
            },
            tooltip: {
              trigger: 'axis',
              axisPointer: {
                type: 'shadow'
              }
            },
            // legend: {
            //   data: data1
            // },
            grid: {
              left: '3%',
              right: '4%',
              bottom: '3%',
              containLabel: true
            },
            xAxis: {
              type: 'category',
              data: data1,
              axisLabel: { interval: 0 }

            },
            yAxis: {

              type: 'value',

            },
            series: [
              {

                type: 'bar',
                data: data2,
                itemStyle: {
                  normal: {
                    color: function (params) {

                      var colorList = ['#dd6b66', '#759aa0', '#e69d87', '#8dc1a9', '#ea7e53', '#eedd78', '#73a373', '#73b9bc', '#7289ab', '#91ca8c', '#f49f42'];
                      return colorList[params.dataIndex]
                    },
                  }
                },
                label: {
                  normal: {
                    show: true,
                    position: 'top'
                  }
                },
              }

            ]
          })






        }).catch(function (error) {
          console.log("error", error);
        })
    },
    drawbar2() {
      console.log('画图2')
      let barchart = this.$echarts.init(this.$refs.barchart2, 'dark')
      axios.get('getAllClassificationCount', {})
        .then((response) => {
          var tt = response.data.results

          console.log(tt)
          var data1 = ['未分级', 'Ⅰ级', 'Ⅱ级', 'Ⅲ级', 'Ⅳ级', 'Ⅴ级']
          var data2 = []
          tt.forEach(
            function (v) {
              data2.push(v.count);

            });


          barchart.setOption({
            title: {
              text: '病人级别统计'
            },
            tooltip: {
              trigger: 'axis',
              axisPointer: {
                type: 'shadow'
              }
            },
            // legend: {
            //   data: data1
            // },
            grid: {
              left: '3%',
              right: '4%',
              bottom: '3%',
              containLabel: true
            },
            xAxis: {
              type: 'category',
              data: data1,
              axisLabel: { interval: 0 }

            },
            yAxis: {

              type: 'value',

            },
            series: [
              {

                type: 'bar',
                data: data2,
                itemStyle: {
                  normal: {
                    color: function (params) {
                      //首先定义一个数组 
                      // var colorList = ["#c23531",
                      //   "#2f4554",
                      //   "#61a0a8",
                      //   "#d48265",
                      //   "#91c7ae",
                      //   "#749f83",
                      //   "#ca8622",
                      //   "#bda29a",
                      //   "#6e7074",
                      //   "#546570",
                      //   "#c4ccd3"]
                      var colorList = ['#dd6b66', '#759aa0', '#e69d87', '#8dc1a9', '#ea7e53', '#eedd78', '#73a373', '#73b9bc', '#7289ab', '#91ca8c', '#f49f42'];
                      return colorList[params.dataIndex]
                    },
                  }
                },
                label: {
                  normal: {
                    show: true,
                    position: 'top'
                  }
                },

              },

            ]
          })






        }).catch(function (error) {
          console.log("error", error);
        })
    }

  }
}
</script>
<style >
/* .overallcss {
  background-color: #c6e2ff;
} */
.picture_header {
  /* background-color: #808288; */
  background-color: #0070a8;
}
.picture_title {
  padding: 2px 15px;
  line-height: 25px;
  font-size: 14px;
  color: white;
  text-align: left;
}
.picture_bodyo {
  border: 1px solid #808288;
  text-align: left;
  background: rgba(51, 102, 153, 0.5);

  /* color: cornflowerblue; */
}
.cardcss1 {
  background-color: #333333;
  color: white;
  font-family: "Helvetica Neue";
  font-size: 20px;
  text-align: center;
}
</style>

