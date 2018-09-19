<template>
  <el-menu class="navbar" mode="horizontal">
    <hamburger class="hamburger-container" :toggleClick="toggleSideBar" :isActive="sidebar.opened"></hamburger>
    <breadcrumb></breadcrumb>

    <el-dropdown class="avatar-container" trigger="click">
      <div class="avatar-wrapper">
        <span>{{avatar}}</span>
        <i class="el-icon-caret-bottom"></i>
      </div>
      <el-dropdown-menu class="user-dropdown" slot="dropdown">
        <el-dropdown-item>
          <span @click="dialogFormVisible = true">手续费设置</span>
        </el-dropdown-item>
        <el-dropdown-item divided>
          <span @click="logout" style="display:block;">退出</span>
        </el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
    <el-dialog  width="600px"  title="转账手续费设置" :visible.sync="dialogFormVisible">
      <el-slider
        v-model="newFee.fee"
        :min="0.0000252"
        :max="0.002016"
        :step="0.000001"
      >
      </el-slider>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="setFee">确 定</el-button>
      </div>
    </el-dialog>
  </el-menu>
</template>

<script>
  import {mapGetters} from 'vuex'
  import Breadcrumb from '@/components/Breadcrumb'
  import Hamburger from '@/components/Hamburger'

  export default {
    data() {
      return {
        newFee: {
          fee: 0.00025
        },
        dialogFormVisible: false
      }
    },
    components: {
      Breadcrumb,
      Hamburger
    },
    watch: {
      'newFee.fee': function(v) {
        this.newFee.gasPrice = parseInt(v / this.feeData.gasLimit * 1000000000000000000)
      }
    },
    computed: {
      ...mapGetters([
        'sidebar',
        'avatar'
      ])
    },
    methods: {
      toggleSideBar() {
        this.$store.dispatch('ToggleSideBar')
      },
      logout() {
        this.$store.dispatch('LogOut').then(() => {
          location.reload() // 为了重新实例化vue-router对象 避免bug
        })
      },
      setFee() {
        this.dialogFormVisible = false
      }
    }
  }
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
  .navbar {
    height: 50px;
    line-height: 50px;
    border-radius: 0 !important;

    .hamburger-container {
      line-height: 58px;
      height: 50px;
      float: left;
      padding: 0 10px;
    }
    .screenfull {
      position: absolute;
      right: 90px;
      top: 16px;
      color: red;
    }
    .avatar-container {
      height: 50px;
      display: inline-block;
      position: absolute;
      right: 35px;
      .avatar-wrapper {
        cursor: pointer;
        margin-top: 5px;
        position: relative;
        .user-avatar {
          width: 40px;
          height: 40px;
          border-radius: 10px;
        }
        .el-icon-caret-bottom {
          position: absolute;
          right: -20px;
          top: 19px;
          font-size: 12px;
        }
      }
    }
  }
</style>

