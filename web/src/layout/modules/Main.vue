<template>
  <el-tabs v-model="activeTab" type="card" @tab-click="clickTab" @tab-remove="removeTab">
    <el-tab-pane
        v-for="(item, index) in tabs"
        :closable="item.isClose"
        :key="index"
        :label="item.name"
        :name="item.name">
      <router-view></router-view>
    </el-tab-pane>
  </el-tabs>
</template>

<script>
import store from "../../store";
import routers from "../../router/routers";
export default {
  name: "Main",
  computed: {
    activeTab: {
      get: function (){
        return store.state.activeIndex
      },
      set: function (val) {
        store.dispatch('activeIndexAction', val)
      }
    },
    tabs: {
      get: function () {
        return store.state.openTabs
      }
    }
  },
  methods: {
    //  点击tab
    clickTab(tab){
      //  跳转到对应的tab
      routers.push({name: tab.name})
    },
    //  移除tab
    removeTab(name){
      //  遍历当前已打开的tabs
      this.tabs.forEach((tab, index) => {
        //  如果关闭的是当前激活状态的tab
        if (tab.name === name){
          //  则将下一个tab设置为激活状态
          //  如果当前激活状态为最后一个,则将上一个tab设置为激活状态
          const nextTab = this.tabs[index + 1] || this.tabs[index - 1]
          if (nextTab){
            store.dispatch('activeIndexAction', nextTab.name)
            //  跳转至当前页面
            routers.push({path: nextTab.path})
          }
        }
      })
      //  在已打开tabs的缓存中删除当前删除的tab
      store.dispatch('removeTabAction', name)
    }
  }
}
</script>

<style scoped>
::v-deep .el-tabs__content{
  height: calc(100vh - 120px);
  padding-left: 30px;
  padding-right: 30px;
}
::v-deep  .el-tabs__nav-wrap{
  height: 40px;
  border-top: 1px solid #d8dce5;
  border-bottom: 1px solid #d8dce5;
  background-color: rgb(247, 247, 247);
}

::v-deep  .el-tabs__nav{
  border: none!important;
  height: 40px;
  display: flex;
  align-items: center;
}

::v-deep .el-tabs__item.is-active {
  height: 30px;
  line-height: 30px;
  background-color: #42b983;
  border-color: #42b983;
  color: white;
}

::v-deep  .el-tabs__item.is-active::before {
  content: "";
  background-color: #fff;
  display: inline-block;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  margin-right: 4px;
}
</style>