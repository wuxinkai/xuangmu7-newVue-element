<template>
  <div class="vuexBox">
    <h3 style="color:red">
      <p>(1) 页面切换数据不会变换</p>
      <p>(2) 页面刷新恢复初始值</p>
    </h3>
    <line-progress :width="'30%'"></line-progress>
    <circle-progress :width="'30%'"></circle-progress>
    <update-vuex :width="'30%'"></update-vuex>
  </div>
</template>

<script>
import LineProgress from './components/LineProgress'
import CircleProgress from './components/CircleProgress'
import UpdateVuex from './components/UpdateVuex'

export default {
  components: {
    LineProgress,
    CircleProgress,
    UpdateVuex
  }
}
</script>

<style lang="less" scoped>
.vuexBox {
  width: 100%;
  min-height: 85%;
  margin: 20px 0;
  border: 1px solid rgba(173, 216, 230, 1);
  background-color: #fff;
  display: flex;
  flex-flow: column wrap;
  justify-content: space-around;
  align-items: center;
}
</style>