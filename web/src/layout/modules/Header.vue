<template>
  <div style="width: 100%;">
    <el-row>
      <el-col :span="12">
        <span>EMS后台管理系统</span>
      </el-col>
      <el-col :span="12">
        <el-dropdown @command="handleCommand">
          <span class="el-dropdown-link">{{username}}<i class="el-icon-arrow-down el-icon--right"></i></span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item command="pwd">修改密码</el-dropdown-item>
            <el-dropdown-item command="logout">退出</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-col>
    </el-row>
    <update-password :dialog-visible.sync="dialogVisible" @logout="clearToken"></update-password>
  </div>
</template>

<script>
import updatePassword from "../../views/user/updatePassword";
import store from "../../store";
import routers from "../../router/routers";
import {infoMsg} from "../../utils/message";
export default {
  name: "Header",
  components: {
    updatePassword
  },
  data(){
    return{
      username: store.state.userInfo.nickName,
      dialogVisible: false
    }
  },
  methods: {
    handleCommand(command){
      if (command === 'logout'){
        this.logout()
      } else if (command === 'pwd'){
        this.dialogVisible = true
      }
    },
    //  退出
    logout(){
      this.$confirm('确定退出当前登录？', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        this.clearToken()
      }).catch(() => {
        infoMsg('操作已取消')
      })
    },
    //  清空token
    clearToken(){
      //  清空token
      store.dispatch('tokenAction', null)
      //  清空refreshToken
      store.dispatch('refreshAction', null)
      //  跳转到登录页面
      routers.push({path: '/login'})
    }
  }
}
</script>

<style scoped>
  .el-row{
    width: 100%;
  }
  .el-dropdown{
    float: right;
  }
  .el-dropdown-link {
    cursor: pointer;
    color: #409EFF;
  }
  .el-icon-arrow-down {
    font-size: 12px;
  }
</style>