<template>
    <Row class="header">
      <i-col span="8" class="header_inco">
        <router-link to="/" replace>
          <Icon type="chevron-left" color="#ffffff"></Icon>
        </router-link>
      </i-col>
      <i-col span="8" class="ccol">本月打卡记录</i-col>
      <i-col span="8" class="rcol"><Icon type="refresh"></Icon></i-col>
      <!--<img src="../../../assets/img/icon_03.png" alt="">-->
    </Row>
</template>

<script>
export default {
  name: 'Header'
}
</script>

<style lang="stylus" scoped>
 .header
    width : 100%
    background : #2680c4
    color : #fff
    font-size 16px
    line-height : 3
    .header_inco
      padding-left 20px
 .ccol
    text-align center
 .rcol{
    text-align right
    padding-right 20px

 }
</style>
