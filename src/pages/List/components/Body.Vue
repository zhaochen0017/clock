<template>
  <div class="main">
    <Row class="body">
      <i-col span="8">日期</i-col>
      <i-col span="8">上班</i-col>
      <i-col span="8">下班</i-col>
    </Row>
    <ul class="list">
      <li v-for="item of dkjl" :key="item.Time"><span>{{item.Time}}</span><span>{{item.AmTime}}</span><span>{{item.PmTime}}</span></li>
    </ul>
  </div>


</template>

<script>
  export default {
    name: 'Header',
    props: {
      dkjl: Array
    }
  }
</script>

<style lang="stylus" scoped>
  .main
    width 100%
    padding-bottom 20px
    .body
      width: 100%
      line-height 3
      font-size 14px
      color #2680c4
      background #eff3fa
      text-align center
    .list
      line-height 3
      font-size 14px
      text-align center
      color #2680c4
    .list li:nth-of-type(2n-1)
      background #fff
    .list li:nth-of-type(2n)
      background #eff3fa
    .list span{
      display inline-block
      width 33.3%

    }
</style>
