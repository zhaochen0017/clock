<style lang="stylus" scoped>
  .body
    width 100%
    line-height 2
    background #fff
    color #323333
    font-size 14px
    text-align center
    .title
      display inline-block
      width: 100%
      padding 20px 0.3% 5px 0.3%
      border-bottom 1px solid #eee
      .title-bold
        font-weight 800
    .title li
      display inline-block
      float left
      width 33.1%
      min-height: 10px
      line-height 1.3
    .cont
      margin 20px 6%
      width 88%
      /*border 1px solid #ddd*/
      text-align left
      font-size 12px
      color #666666
      .left-icon
        display inline-block
        margin 10px 20px 0 0
        width:7px
        height:180px
        float: left
        .leftline
          display: inline-block
          height: 217px;
          border-right: 1px solid #ccc;
          width: 4px;
          float: left;
          z-index 2
        .icon-circle
          display inline-block
          width 7px
          height 7px
          background #2680c4
          border-radius 50%
          float left
          z-index 10
        .circleactive
          background #b3b2b2
      .bigmsg
        font-size 16px
        color #4c4c4c
        font-weight bold
      .ml20
        margin-left 20px
      .btn-box
        width 105px
        margin 0 auto
        .btn-cricle
          text-align center
          display inline-block
          width 105px
          height 105px
          margin 15px auto 30px auto
          color #fff
          background: -webkit-linear-gradient(#6ab0e1, #358dcd); /* Safari 5.1 - 6.0 */
          background: -o-linear-gradient(#6ab0e1, #358dcd); /* Opera 11.1 - 12.0 */
          background: -moz-linear-gradient(#6ab0e1, #358dcd); /* Firefox 3.6 - 15 */
          background: linear-gradient(#6ab0e1, #358dcd); /* 标准的语法 */
          box-shadow 5px 5px 15px #6ab0e1
          border-radius 50%
          line-height 1.5
          box-shadow 5px 5px 15px #c8c5c5
          p
            margin-top 35px
            font-size 16px
            /*color #fff*/
        .active
          color #666666
          background: -webkit-linear-gradient(#e7e7e6, #cccccc); /* Safari 5.1 - 6.0 */
          background: -o-linear-gradient(#e7e7e6, #ccc); /* Opera 11.1 - 12.0 */
          background: -moz-linear-gradient(#e7e7e6, #ccc); /* Firefox 3.6 - 15 */
          background: linear-gradient(#e7e7e6, #ccc); /* 标准的语法 */
    .bluetext
      color #2680c4
      font-size 12px
    .btn
      width 80%
      background #2680c4
      color #fff
      font-size 14px
      line-height 2
      margin-bottom 50px
</style>
<template>
   <div class="body">
     <ul class="title">
       <li class="title-bold">{{userName?userName:'未获得数据'}}</li>
       <li> </li>
       <li class="title-bold">{{week}}</li>
       <li>{{userId?userId:'未获得数据'}}</li>
       <li></li>
       <li>{{time}}</li>
     </ul>
     <div class="cont">
       <div class="left-icon">
         <span class="icon-circle" :class="{circleactive:hasClockAm}"></span>
         <span class="leftline"></span>
         <span class="icon-circle " :class="{circleactive:hasClockPm}"></span>
       </div>
       <p class="ml20">上班时间 9:00</p>
       <p class="bigmsg ml20">打卡时间：{{amTime?amTime:'未打卡'}}</p>
       <div class="btn-box">
         <div class="btn-cricle" @click="playCard(hasClockAm)" :class="{active:hasClockAm}">
           <p>上班打卡</p>
           <time>{{hasClockAm ? '已打卡':now}}</time>
         </div>
       </div>
       <p class="ml20">下班时间 18:00</p>
       <p class="bigmsg ml20">打卡时间：{{pmTime?pmTime:'未打卡'}}</p>
       <div class="btn-box">
         <div class="btn-cricle" :class="{active:hasClockPm}" @click="playCard(hasClockPm)">
           <p>下班打卡</p>
           <time>{{now}}</time>
         </div>
       </div>
     </div>
     <p class="bluetext">仅支持WiFi打卡</p>
     <router-link :to="{path:'/list',query:{userId:userId}}">
        <Button class="btn" >查看本月考勤</Button>
     </router-link>
  </div>
</template>
<script>
import axios from 'axios'
import thisisjsonp  from 'jsonp'
export default {
  name: 'Body',
  data () {
    return {
      myDate : new Date(),
      hasClockAm:false,
      hasClockPm:false,
      amTime:"",
      pmTime:"",
      userId : "",
      userName : "",
      userIp : "",
      userImei : "",
      userType : ""
    }
  },
  mounted : function () {
    //加载壳子参数
    this.initMessage();
    //定时获取当前时间
    var _this = this;
    setInterval(function() {
      _this.myDate = new Date();
    }, 300);
  },
  methods : {
    //加载壳子参数
    initMessage () {
        //设置头部
        SmartOffice.invoke('setTitleBgColor','{"color":"#FF0000"}','')
        SmartOffice.invoke('setTitle','{"name":"考勤打卡"}','')

        //获取type和ip
        var this_ = this;
        SmartOffice.invoke("getNetworkInfo","",callbackWork)
        function callbackWork (res) {
          var object1 = eval('(' + res + ')');
          this_.userType = object1.type;
          this_.userIp = object1.ip;
        }

        //获取imei
        SmartOffice.invoke("getDeviceInfo","",callbackDevice)
        function callbackDevice (res) {
          var object1 = eval('(' + res + ')');
          this_.userImei = object1.imei;
        }

        //获取name和userid
        SmartOffice.invoke("getUserInfo","",callbackUser)
        function callbackUser (res) {
          var object1 = eval('(' + res + ')');
          this_.userName = object1.name;
          this_.userId = object1.employeeId;
          this_.getIndexInfo();
        }
    },
    getIndexInfo () {
      axios.get('http://10.128.36.34:888/MobileSignIn.ashx?act=todayjson&empNo='+this.userId).then(this.getIndexInfoSucc)
    },
    getIndexInfoSucc (res) {
      res = res.data
      this.today = res
      var temp1 = res.checkTimeAM
      var temp2 = res.checkTimePM

      if(!temp1&&!temp2){
        this.hasClockPm = true
      }
      if(temp1){
        this.hasClockAm=true
        temp1 = temp1.split(" ")[1];
        this.amTime = temp1
      }
      if(temp2){
        this.hasClockAm=true
        temp2 = temp2.split(" ")[1];
        this.pmTime = temp2
      }
    },
    playCard : function (isClick) {
      if(isClick){
        return
      }else {
        if(this.check()){
            var myUrl = 'http://10.128.36.34:888/MobileSignIn.ashx?act=add&empNo='+this.userId+'&imei='+this.userImei
            axios.get(myUrl).then(this.getPlayCardSucc)
          }
      }
    },
    getPlayCardSucc : function (res) {
      this.$Message.success(res.data);
      this.getIndexInfo();
    },
    check : function () {
      //判断当前是否使用wifi
      if(this.userType.indexOf("WIFI") > -1){
        var ip = "192.144.37.228";
        ip =this.userIp;
        var patt = /^10\.(144|2|104|53|134)\.(\d+)\.(\d+)$/;
        if(patt.test(ip)){
          return true;
        }else{
          this.$Message.success("您用的不是公司wifi");
          return false;
        }
      }else{
        this.$Message.success("请使用公司wifi进行打卡");
        return false;
      }
      return false;
    }
  },
  computed : {
    week : function () {
      var a = new Array("日", "一", "二", "三", "四", "五", "六");
      var i = a[this.myDate.getDay()];
      return `星期${i}`;
    },
    time : function () {
       var year=this.myDate.getFullYear();    //获取完整的年份(4位,1970-????)
       var mouth=this.myDate.getMonth()+1;       //获取当前月份(0-11,0代表1月)
       var day=this.myDate.getDate();        //获取当前日(1-31)
       return `${year}.${mouth}.${day}`;
    },
    now : function () {
       var hours= this.myDate.getHours();       //获取当前小时数(0-23)
       var min= this.myDate.getMinutes();     //获取当前分钟数(0-59)
       var second= this.myDate.getSeconds();     //获取当前分钟数(0-59)
       return`${hours}:${min}:${second}`;
    }
  }
}
</script>


