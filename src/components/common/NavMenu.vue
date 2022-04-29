<template>
  <div>
    <el-menu
      :default-active="currentPath"
      router
      mode="horizontal"
      background-color="white"
      text-color="#222"
      active-text-color="red"
      style="min-width: 1300px">
      <el-menu-item v-for="(item,i) in navList" :key="i" :index="item.name">
        {{ item.navItem }}
      </el-menu-item>
      <span style="position: absolute;padding-top: 20px;right: 43%;font-size: 20px;font-weight: bold">White Jotter - Your Mind Palace</span>
      <!-- <el-input
        placeholder="快速搜索..."
        prefix-icon="el-icon-search"
        size="medium"
        style="width: 300px;position:absolute;margin-top: 12px;right: 18%"
        v-model="keywords">
      </el-input> -->
        
       <el-col style="width: 300px;height:100%;position:absolute;right: 0">
  
          <div class="avatar-box" v-if="username">
              
              <i class="el-icon-message-solid" style="margin-top:20px;"></i>
              <el-dropdown style="margin-top:7px;">
              <el-button icon="el-icon-user-solid" plain type="text">
                username
              </el-button>
              
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item >个人主页  </el-dropdown-item>
                <el-dropdown-item @click.native="logout">退出</el-dropdown-item>
               
              </el-dropdown-menu>
            </el-dropdown>
              <!-- <el-dropdown class="ava-name-box" trigger="click">
                <el-avatar icon="el-icon-user-solid" style="margin-top:10px;margin-left:30px;" class="el-dropdown-link"></el-avatar>
                <span class="el-dropdown-link" style="margin-top:20px;margin-left:10px;">username<i class="el-icon-arrow-down el-icon--right"></i></span>
                
                  <el-dropdown-menu slot="dropdown">
                    <el-dropdown-item>个人主页</el-dropdown-item>
                    <el-dropdown-item>退出</el-dropdown-item>
                  </el-dropdown-menu>
              </el-dropdown> -->
              
          </div>
          
    
          <div style="margin-top:20px;" v-else>
            <el-link href="/login" :underline="false" target="_blank" class="add-link">登录</el-link>
            <el-link href="/register" :underline="false" target="_blank" class="add-link" style="margin-left:10px;">注册</el-link>
          </div>
        
      </el-col>
   
    </el-menu>
    
  </div>
</template>

<script>
import store from '../../store'
  export default {
    name: 'NavMenu',

    data () {
      return {
        username:store.state.username,
        navList: [
          {name: '/index', navItem: '首页'},
          {name: '/jotter', navItem: '组队'},
          {name: '/library', navItem: '图书馆'},
          {name: '/login', navItem: '管理中心'}
        ],
        keywords: ''
      }
    },
    computed: {
      hoverBackground () {
        return '#ffd04b'
      },
      currentPath () {
        var x = this.$route.path.indexOf('/', 1)
        if (x !== -1) {
          return this.$route.path.substring(0, x)
        } else {
          return this.$route.path
        }
      }
    },
    methods: {
      logout () {
        var _this = this
        this.$axios.get('/logout').then(resp => {
          if (resp && resp.data.code === 200) {
            _this.$store.commit('logout')
            _this.$router.replace('/index')
            // 清空路由，防止路由重复加载
            const newRouter = createRouter()
            _this.$router.matcher = newRouter.matcher
          }
        }).catch(failResponse => {})
      }
    }
  }
</script>

<style scoped>
  a{
    text-decoration: none;
  }

  span {
    pointer-events: none;
  }
  .avatar-box {
    height: 100%;
    display:flex;
    flex-direction: row;
    justify-content: center;
  }
  .ava-name-box{
    display:flex;
    flex-direction: row;
    justify-content: center;
  }

</style>
