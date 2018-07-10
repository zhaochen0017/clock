<template>
  <div class="list">
    <Body :dkjl="dkjl"></Body>
  </div>
</template>

<script>

import Body from './components/Body'
import axios from 'axios'
export default {
  name: 'List',
  components: {
    Body
  },
  data () {
    return {
      dkjl : [],
    }
  },
  methods: {
    getIndexInfo () {
      var userId = this.$route.query.userId
      axios.get('http://10.128.36.34:888/MobileSignIn.ashx?act=listjson&empNo='+userId).then(this.getIndexInfoSucc)
    },
    getIndexInfoSucc (res) {
      this.dkjl = res.data;
    }
  },
  mounted () {
    this.getIndexInfo();
  }
}
</script>

<style scoped>
  .list{
    height: 100%;
    background: #eff3fa;
  }
</style>
