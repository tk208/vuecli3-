<template>
  <div class="index">
      <router-view></router-view>
      <TabBar :data="tabBarData" />
  </div>
</template>

<script>
import TabBar from '../components/TabBar'
export default {
    name: "Index",
    data(){
      return {
        tabBarData: [
          {title: '首页', icon: 'home', path: '/home'},
          {title: '订单', icon: 'file-text-o', path: '/order'},
          {title: '我的', icon: 'user', path: '/me'},
        ]
      }
    },
    components: {
      TabBar
    }
}
</script>

<style scoped>
  .index {
    width: 100%;
    height: calc(100% - 45px);
  }
</style>