﻿<template>
  <div>

    <img src="../assets/header.png">

    <div class="backcolor1">
      <el-row style="padding-top:10px;">
        <el-col :span="22"
                :offset="1">

          <el-row>

            <el-col :span="8">

              <div v-if="showass">
                <video id="501"
                       playsinline=""
                       width="450"
                       height="400"
                       autoplay
                       controls
                       poster="../assets/tx1.jpg"
                       style="object-fit:cover">
                </video>
                <div class="text-v">
                  现场通话
                </div>
              </div>
              <div v-else>
                <video id="Android11"
                       controls
                       autoplay
                       width="450"
                       height="400"
                       poster="../assets/tx1.jpg"
                       playsinline="">
                </video>
                <div class="text-v">
                  AR眼镜
                </div>
              </div>

            </el-col>
            <el-col :span="8">

              <video id="502"
                     playsinline=""
                     width="450"
                     height="400"
                     poster="../assets/tx1.jpg"
                     autoplay
                     controls
                     style="object-fit:cover">
              </video>
              <div class="text-v">
                车辆通话
              </div>
            </el-col>
            <el-col :span="8">

              <video id="310"
                     playsinline=""
                     width="450"
                     height="400"
                     poster="../assets/tx1.jpg"
                     autoplay
                     controls
                     style="object-fit:cover">
              </video>
              <div class="text-v">
                医院通话
              </div>

            </el-col>

          </el-row>
          <el-row style="margin-top:15px;margin-bottom:15px;">

            <el-col :span="8">
             <!--  <video id="myPlayer"
                     width="450"
                     height="400"
                     controls
                     autoplay
                     poster="../assets/tx1.jpg"
                     webkit-playsinline>

                <source src="http://hls01open.ys7.com/openlive/b61dd31246a441179438f5bdf093cfe5.m3u8" />

              </video> -->
              <video id="504"
                     playsinline=""
                     width="450"
                     height="400"
                     poster="../assets/tx1.jpg"
                     autoplay
                     controls
                     style="object-fit:cover">
              </video>

              <div class="text-v">
                现场通话
              </div>
            </el-col>
            <el-col :span="8">
              <video id="901"
                     playsinline=""
                     width="450"
                     height="400"
                     poster="../assets/tx1.jpg"
                     autoplay
                     controls
                     style="object-fit:cover">
              </video>
              <div class="text-v">
                指 挥
              </div>
              <!-- <video id="myPlayer2"
                       width="450"
                       height="400"
                       controls
                       autoplay
                       poster="/static/media/sky.jpg"
                       webkit-playsinline>

                  <source src="http://hls01open.ys7.com/openlive/95e93e36c95d44299a66e9d891275360.hd.m3u8" />
                </video>
                <div style="padding: 14px;font-size:20px;">
                  <span>车辆实况</span>
                </div> -->
            </el-col>
            <el-col :span="8">
              <video id="401"
                     playsinline=""
                     width="450"
                     height="400"
                     poster="../assets/tx1.jpg"
                     controls
                     autoplay
                     style="object-fit:cover">
              </video>
              <div class="text-v">
                专 家
              </div>
              <!-- <video id="local"
                 height="0">
          </video> -->
            </el-col>
          </el-row>
        </el-col>
        <el-col :span="1">
          <video id="local"
                 height="0">
          </video>
        </el-col>

      </el-row>
    </div>

  </div>
</template>
<script>
import scriptjs from 'scriptjs'
import axios from 'axios';
import global from './global.vue'


export default {

  components: {


  },
  sockets: {


  },


  data() {
    return {
      // src1: "..assets/mda-ihutmh8752y9rdbw.mp4"
      src1: '/static/media/mda-ihutmh8752y9rdbw.mp4',
      //判断现场还是眼镜 GYX 19/9/14
      showass: true,
      showcar: true,

      showPatinfo: false,
      perinfo: {},
      ExpertList: [],
      choosenpatient: '',
      showPMH: false,
      //消息列表展示的消息类型 A 病人B会场 无
      type: '',
      curpatient: '',
      curhelp: '',
      //以下是既往病历





    }
  },
  watch: {
    showass: function (val, oldval) {
      console.log(val, '变化一次', '监听')
    },
    showcar: function (val, oldval) {
      console.log(val, '变化一次', '监听')
    },
  },
  mounted() {

    // var player = new EZuikit.EZUIPlayer('myPlayer');
    // var player2 = new EZuikit.EZUIPlayer('myPlayer2');
    // console.log(this.$route.query.id)
    // console.log(this.$route.params.id)

    window.configData = {
      "sdkappid": 1400203281,
      "users": [{
        "userId": "web01",
        "userToken": "eJxlz11PgzAUgOF7fgXprUbbQufYHR*SMCE6p9vYTVNowapjTakyNP53Iy4ZxnP7vCcn59OybRs8pMsLVpb7t8ZQ0ysB7JkNIDg-oVKSU2aoo-k-FAcltaCsMkIPiAghGMJxI7lojKzksehEAdGIW-5Chxu-*y6EGDp4*ieR9YDZdR4mi*jQdtC7ytoofu5RzEyt0srvwnC7DrgIG3y3ine4WMbadMmTn-TBFofJfHObN5dnN6lXO*66v48eWbwpc7KaFv485yz4eM1GJ43cieNDE*Riz0OTkb4L3cp9MwQYIoKwA38GWF-WN6DjXFA_"
      }]
    }


    WebRTCAPI.fn.detectRTC({
      screenshare: true
    }, function (info) {
      if (!info.support) {
        alert('不支持WebRTC')
      } else {
        // alert('支持WebRTC')
      }
    });
    var _mtac = { "senseHash": 0 };
    (function () {
      var mta = document.createElement("script");
      mta.src = "//pingjs.qq.com/h5/stats.js";
      mta.setAttribute("name", "MTAH5");
      mta.setAttribute("sid", "500538821");
      mta.setAttribute("cid", "500538834");
      var s = document.getElementsByTagName("script")[0];
      s.parentNode.insertBefore(mta, s);
    })();

    window.dataLayer = window.dataLayer || [];
    function gtag() {
      dataLayer.push(arguments);
    }
    gtag('js', new Date());
    gtag('config', 'UA-121844058-1');
    // do your logic.
    // 实例化，绑定事件等操作
    var sdkappid = configData.sdkappid;
    var users = configData.users;


    // window.onload = function () {
    //   push();
    // }

    // 渲染出用户
    (function render() {
      var html = '';
      users.forEach(user => {
        html += '<option value="' + user.userId + '">' + user.userId + '</option>';
      });
      $('#userId').html(html);
    })();
    //页面加载前预登录预推流
    push();


    function onKickout() {
      alert("on kick out!");
    }

    function quitRTC() {
      stopRTC();
      RTC.quit();
      window.close(); //关闭当前页面
      $("#video-section").hide();
      $("#input-container").show();
      $("#remote-video-wrap").html("");
    }

    function onRelayTimeout(msg) {
      alert("onRelayTimeout!" + (msg ? JSON.stringify(msg) : ""));
    }
    //禁止进入陌生人
    function createVideoElement(id, isLocal) {
      console.log('开始创建div')
      var videoDiv = document.createElement("div");

      videoDiv.innerHTML = '<video id="' + id + '" autoplay ' + (isLocal ? 'muted' : '') + ' playsinline ></source=>';

      document.querySelector("#remote-video-wrap").appendChild(videoDiv);

      return document.getElementById(id);
    }

    function onLocalStreamAdd(info) {
      if (info.stream && info.stream.active === true) {
        var id = "local";
        var video = document.getElementById(id);
        console.log('本地接入')
        if (!video) {
          createVideoElement(id, true);
        }
        var video = document.getElementById(id)
        video.srcObject = info.stream;
        video.muted = true
        video.autoplay = true
        video.playsinline = true
      }
    }
    var that = this
    function onRemoteStreamUpdate(info) {
      console.debug(info)
      console.log(this)

      console.log('远程视频接入')
      // console.log(info.userId, info.videoId)

      if (info.stream && info.stream.active === true) {
        var id = info.userId;
        console.log(id);
        //这里判断是现场通话还是AR眼镜 GYX 19/9/14
        if (id == "Android11") {
          that.showass = false
          console.log('出现了AR')
        }
        else if (id == "501") {
          that.showass = true
          console.log('出现了现场通话')
        }
        else if (id == "503") {
          id = "310"
        }
        else if (id == "503") {
          id = "310"
        }

        else if (id.substring(0,1)=='1') {
          //如果id901 就把它改成901
          id = "501"
        }
        else if (id.substring(0,1)=='2') {
          //如果id901 就把它改成901
          id = "502"
        }
        else if (id.substring(0,1)=='3') {
          //如果id901 就把它改成901
          id = "310"
        }
else if (id == "902") {
          //如果id901 就把它改成901
          id = "901"
        }
 else if (id == "903") {
          //如果id903 就把它改成901
          id = "901"
        }
 else if (id == "904") {
          //如果id904 就把它改成901
          id = "901"
        } else if (id == "905") {
          //如果id905 就把它改成901
          id = "901"
        }
 else if (id == "130") {
          //如果id901 就把它改成902
          id = "901"
        }
         else if (id == "402") {
          //如果id901 就把它改成902
          id = "401"
        }
        else {
          console.log('啥也没出现')
        }


        // console.log(this.showass, '视频后')
        var video = document.getElementById(id);
        if (!video) {
          console.log('禁止陌生人加入')
        }
        else {
          video.srcObject = info.stream;
        }





      } else {
        // console.log('欢迎用户' + info.userId + '加入房间');
      }
    }

    function onRemoteStreamRemove(info) {
      // console.log(info.userId + ' 断开了连接');
      var videoNode = document.getElementById(info.userId);
      if (videoNode) {
        videoNode.srcObject = null;
        // document.getElementById(info.videoId).parentElement.removeChild(videoNode);
        document.getElementById(info.userId).parentElement
      }
    }

    function onWebSocketClose() {
      RTC.quit();
    }

    function gotStream(opt, succ) {
      RTC.getLocalStream({
        video: false,
        audio: true,
        videoDevice: opt.videoDevice,
        // 如需指定分辨率，可以在attributes中增加对width和height的约束
        // 否则将获取摄像头的默认分辨率
        // 更多配置项 请参考 接口API
        // https://cloud.tencent.com/document/product/647/17251#webrtcapi.getlocalstream
        // attributes: {
        //   width: 640,
        //   height: 320
        // }
      }, function (info) {
        var stream = info.stream;
        succ(stream)
      });
    }


    function initRTC(opts) {
      window.RTC = new WebRTCAPI({
        userId: opts.userId,
        userSig: opts.userSig,
        sdkAppId: opts.sdkappid,
        accountType: opts.accountType
      }, function () {
        RTC.enterRoom({
          roomid: opts.roomid * 1,
          privateMapKey: opts.privateMapKey,
          role: "user",
        }, function (info) {
          console.warn("init succ", info)
          gotStream({
            audio: true,
            video: false
          }, function (stream) {
            RTC.startRTC({
              stream: stream,
              role: 'user'
            });
          });
          startRTC();
        }, function (error) {
          console.error("init error", error)
        });
      }, function (error) {
        // console.warn("init error", error)
      });

      // 远端流新增/更新
      RTC.on("onRemoteStreamUpdate", onRemoteStreamUpdate)
      // 本地流新增
      RTC.on("onLocalStreamAdd", onLocalStreamAdd)
      // 远端流断开
      RTC.on("onRemoteStreamRemove", onRemoteStreamRemove)
      // 重复登录被T
      RTC.on("onKickout", onKickout)
      // 服务器超时
      RTC.on("onRelayTimeout", onRelayTimeout)

      RTC.on("onErrorNotify", function (info) {
        console.error(info)
        if (info.errorCode === RTC.getErrorCode().GET_LOCAL_CANDIDATE_FAILED) {
          // alert( info.errorMsg )
        }
      });
      RTC.on("onStreamNotify", function (info) {
        // console.warn('onStreamNotify', info)
      });
      RTC.on("onWebSocketNotify", function (info) {
        // console.warn('onWebSocketNotify', info)
      });
      RTC.on("onUserDefinedWebRTCEventNotice", function (info) {
        // console.error( 'onUserDefinedWebRTCEventNotice',info )
      });
    }

    function push() {
      //默认登录了
      // var roomid = $('#roomid').val();
      // var userId = $('#userId').val();

      var roomid = '999'
      var userId = 'web01';

      var userSig = findUserToken(userId);

      $('#c_roomid').html(roomid);
      $('#c_userid').html(userId);

      // 页面处理，显示视频流页面
      $("#video-section").show();
      $("#input-container").hide();

      initRTC({
        "userId": userId,
        "userSig": userSig,
        "sdkappid": sdkappid,
        "accountType": 1, // 随便传一个值，现在没有啥用处
        "roomid": roomid
      });
    }

    function audience() {
      login(true);
    }

    function stopRTC() {
      RTC.stopRTC(0, function (info) {
        // console.debug(info)
      }, function (info) {
        // console.debug(info)
      });
    }

    function stopWs() {
      RTC.global.websocket.close();
    }

    function startRTC() {
      RTC.startRTC(0, function (info) {
        console.debug('success', info)
      }, function (info) {
        console.debug('failed', info)
      });
    }

    function findUserToken(userid) {
      var userToken = null;
      for (var i = 0, len = users.length; i < len; i++) {
        var user = users[i];
        if (user.userId === userid) {
          userToken = user.userToken;
          break;
        }
      }
      return userToken;
    }




  },
  destoryed() {
    // 解绑全局事件
    // 销毁实例
  },

  methods: {
    sendinfo() {
      axios.post('/pushVideoAll', {
      }
      )
        .then((response) => {
          console.log('yitongzhi')
        }).catch(function (error) {
          console.log("error", error);
        })

    },
    getPatientInfo(val) {
      axios.post('getPatientInfo', {
        "patientId": val
      }).then(res => {
        console.log(res.data.results[0])
        // var cdata = 'qioalelimam'
        var tt = res.data.results[0]
        // this.choosenpatient = row.PatientId
        this.perinfo = tt
        this.curpatient = tt
        this.type = 'A'
        this.shake($("#myexample"), "red", 4)

      }).catch(function (err) {
        console.log("error", err)
      })
    },
    handleClose(done) {
      done();
    },
    handleClose2(done) {
      done();
    },
    //查看既往病史
    // viewPMH(ss) {
    //   this.showPMH = true




    // },












    shake(ele, cls, times) {//边框闪烁   
      //ele要闪动的元素 cls闪动的类 times 闪动几次  
      var i = 0, t = false, o = ele.attr("class") + " ", c = "", times = times || 2;
      if (t) return;
      t = setInterval(function () {        i++; c = i % 2 ? o + cls : o; ele.attr("class", c);
        if (i == 2 * times) { clearInterval(t); ele.removeClass(cls); }      }, 200);
    },
    getExpertList() {
      console.log('推送专家')
      this.showPatinfo = true
      axios.get('/getExpertList', {

      }
      )
        .then((response) => {

          this.ExpertList = response.data.results

        }).catch(function (error) {
          console.log("error", error);
        })

    },
    //将病人推送给医生
    ptoe(index) {

      axios.post('/newExpertPatient', {
        // "token": this.GLOBAL.token,
        "patientId": this.perinfo.PatientId,
        "expertId": this.ExpertList[index].PersonnelID
      })
        .then(response => {

          this.$message('推送成功')
        })
        .catch(function (error) {
          console.log("error", error);
        });

    },


    getGroupInfo(val) {
      axios.post('getGroupInfo', {
        "groupNo": val
      }).then(res => {
        console.log(res.data.results[0])

        var tt = res.data.results[0]


        this.curhelp = tt

        this.type = 'B'
        this.shake($("#myexample"), "red", 4)


      }).catch(function (err) {
        console.log("error", err)
      })
    },
    tuisong() {
      console.log('推送')
    },
    quitRTC() {
      this.stopRTC();
      RTC.quit();
      window.close(); //关闭当前页面
      $("#video-section").hide();
      $("#input-container").show();
      $("#remote-video-wrap").html("");
    },
    stopRTC() {
      RTC.stopRTC(0, function (info) {
        // console.debug(info)
      }, function (info) {
        // console.debug(info)
      });
    }


    // 返回一些有用的函数
  }
}
</script>
<style>
.backcolor1 {
  color: white;
}
.text-v {
  font-size: 25px;
  /* font-weight: bold; */
  margin-top: 5px;
}
/* 自定义样式 */
</style>


