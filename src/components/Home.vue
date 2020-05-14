<template>
  <el-container class="home-container">
    <el-header>
      <div>
        <img src="../assets/heima.png" alt=""/>
        <span>电商后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <el-container>
      <el-aside :width="isCollapsed ? '64px' : '200px'">
        <div class="toogle-button" @click="toggleCollaspse">|||</div>
        <!--                侧边栏菜单区-->
        <el-menu background-color="#333744" text-color="#fff" active-text-color="#409EFF" unique-opened
                 :collapse="isCollapsed" :collapse-transition="false" router :default-active="activcPath">
          <!--                    一级菜单-->
          <el-submenu :index="item.id + ''" v-for="item in menulist" :key="item.id">
            <!--                        一级菜单模板区-->
            <template slot="title">
              <!--                            图标-->
              <i :class="iconsObj[item.id]"></i>
              <!--                            文本-->
              <span>{{ item.authName }}</span>
            </template>
            <el-menu-item :index="'/' + subItem.path" v-for="subItem in item.children" :key="subItem.id"
                          @click="saveNavState('/' + subItem.path)">
              <!--                        二级菜单模板区-->
              <template slot="title">
                <!--                            图标-->
                <i class="el-icon-menu"></i>
                <!--                            文本-->
                <span>{{ subItem.authName }}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main>
        <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  created () {
    this.getMenuList()
    this.activcPath = window.sessionStorage.getItem('activePath')
  },
  data () {
    return {
      menulist: [],
      iconsObj: {
        125: 'iconfont icon-user',
        103: 'iconfont icon-tijikongjian',
        101: 'iconfont icon-shangpin',
        102: 'iconfont icon-danju',
        145: 'iconfont icon-baobiao'
      },
      isCollapsed: false,
      activcPath: ''
    }
  },
  name: 'Home',
  methods: {
    logout () {
      window.sessionStorage.clear()
      this.$router.push('/login')
    },
    async getMenuList () {
      const { data: res } = await this.$http.get('menus')
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menulist = res.data
    },
    toggleCollaspse () {
      this.isCollapsed = !this.isCollapsed
    },
    saveNavState (activepath) {
      window.sessionStorage.setItem('activePath', activepath)
      this.activcPath = activepath
    }
  }
}
</script>

<style scoped lang="less">
  .home-container {
    height: 100%;

    .el-header {
      display: flex;
      justify-content: space-between;
      background-color: #373d41;
      padding-left: 0;
      align-items: center;
      color: #fff;
      font-size: 20px;

      > div {
        display: flex;
        align-items: center;

        span {
          margin-left: 15px;
        }
      }
    }

    .el-aside {
      background-color: #333744;

      .el-menu {
        border-right: none;
      }

      .toogle-button {
        background-color: #4a5064;
        font-size: 10px;
        line-height: 24px;
        text-align: center;
        color: #fff;
        letter-spacing: 0.2em;
        cursor: pointer;
      }

      .iconfont {
        margin-right: 10px;
      }
    }

    .el-main {
      background-color: #eaedf1;
    }
  }
</style>
