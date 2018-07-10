<template>
  <div class="wai">
    <Body></Body>
  </div>
</template>

<script>
import Body from './components/Body'
import axios from 'axios'
export default {
  name: 'Home',
	components: {
    Body
  },
  data () {
    return {

    }
  },
  mounted () {
    var qs = this.getQueryString();
    //JdAgEAAoGBAOdC92
    var timestamp = qs["timestamp"]
    var nonce = qs["nonce"]
    var AppSecret = "JdAgEAAoGBAOdC92"
    var sign = qs["sign"]
    var hex_sha1_string = hex_sha1(timestamp+AppSecret+nonce)
    if(sign==hex_sha1_string){
      return
    }else{
      this.$router.push({path: '/error'});
    }


  },
  methods: {
    getQueryString() {
      var qs = location.search.substr(1), // 获取url中"?"符后的字串a
      args = {}, // 保存参数数据的对象
      items = qs.length ? qs.split("&") : [], // 取得每一个参数项,
      item = null,
      len = items.length;

      for(var i = 0; i < len; i++) {
        item = items[i].split("=");
        var name = decodeURIComponent(item[0]),
            value = decodeURIComponent(item[1]);
        if(name) {
          args[name] = value;
        }
      }
      return args;
    }
  }
}
</script>

<style lang="stylus" scoped>
  html,body
    height 100%
    font-size 20px
    .wai
      width 100%
      height 100%
      background #fff
</style>

