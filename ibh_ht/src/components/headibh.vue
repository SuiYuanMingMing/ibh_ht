<template>
  <div class="head">
    <el-menu theme="dark" :default-active="activeIndex" class="el-menu-demo" mode="horizontal">
      <span class="logo"><img src="../assets/logo.png"></span>
      <el-submenu class="login" index="1">
        <template slot="title"><i class="el-icon-my-user"></i>{{ Nickname }}</template>
        <el-menu-item index="2-1" @click="member">个人资料</el-menu-item>
        <el-menu-item index="2-2" @click="loginOut">登出</el-menu-item>
      </el-submenu>
    </el-menu>
  </div>
</template>

<script>
export default {
  name: 'head',
  data () {
    return {
      activeIndex: '1'
    }
  },
  computed: {
    Nickname: function () {
      return this.$store.state.sign.Nickname
    }
  },
  methods: {
    loginOut: function () {
      const self = this
      self.$axios.get('Login/Logout?Id=' + self.$store.state.sign.Id).then(function (response) {
        self.$store.commit('UPDATA_CHANGESTATE', 0)
        self.$notify({
          title: '成功',
          message: '注销成功',
          type: 'success'
        })
        self.$router.push('/')
      }).catch(function (response) {
        console.log(response.response)
      })
    },
    member: function () {
      const self = this
      self.$axios.get('Login/CheckLogin?id=' + self.$store.state.sign.Id).then(function (response) {
        if (response.data) {
          self.$router.push('/member')
        } else {
          self.$store.commit('UPDATA_CHANGESTATE', 0)
          self.$router.push('/#/')
        }
      }).catch(function (response) {
        console.log(response.response)
      })
    }
  }
}
</script>

<style lang="less" scoped>
  .head {
    height:10%;
    width: 100%;
    background: #324157;
  }
  .logo {
    width: 15%;
    height:10%;
    text-align: center;
    float: left;
    img {
      max-height: 50px;
      margin-top: 5px;
    }
  }
  .login {
    float: right;
    margin-right: 10%;
  }
</style>