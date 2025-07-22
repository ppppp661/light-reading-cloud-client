<template>
  <v-app id="inspire" cols="12" sm="6" offset-sm="3">
    <v-app-bar
      app
      dark
      cols="12"
      color="#14B0ED"
      src="@/assets/backgroundImg.png"
      elevation="24"
      dense
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title>轻松阅读</v-toolbar-title>
      <v-spacer />
      <v-btn icon @click="$router.push('/search')">
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <template v-slot:extension>
        <v-tabs align-with-title>
          <v-tab @click="currentTab = 'selected'">精选</v-tab>
          <v-tab @click="currentTab = 'boy'">男生</v-tab>
          <v-tab @click="currentTab = 'gril'">女生</v-tab>
          <v-tab @click="currentTab = 'original'">原创</v-tab>
        </v-tabs>
      </template>
    </v-app-bar>

    <v-content
      class="mx-auto"
      style="
        background: url('@/assets/backgroundImg.png') no-repeat center center;
        background-size: cover;
        min-height: calc(100vh - 64px); /* 减去顶部导航栏高度 */
      "
    >
      <keep-alive>
        <component v-bind:is="currentTabComponent"></component>
      </keep-alive>
    </v-content>

    <!-- 抽屉菜单 -->
    <v-navigation-drawer v-model="drawer" app right>
      <template v-slot:prepend v-if="user">
        <v-list-item two-line>
          <v-list-item-avatar>
            <img :src="user.headImgUrl" />
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title>{{ user.nickName }}</v-list-item-title>
            <v-list-item-subtitle @click="commandHandler('logout')"
              >注销</v-list-item-subtitle
            >
          </v-list-item-content>
        </v-list-item>
      </template>
      <template v-slot:prepend v-else>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title
              ><span @click="commandHandler('login')">登录</span>
              /
              <span @click="commandHandler('register')"
                >注册</span
              ></v-list-item-title
            >
          </v-list-item-content>
        </v-list-item>
      </template>

      <v-divider></v-divider>

      <v-list dense rounded>
        <v-list-item link @click="commandHandler('home')">
          <v-list-item-action>
            <v-icon>mdi-home</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>主页</v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <v-list-item link @click="commandHandler('my')">
          <v-list-item-action>
            <v-icon>mdi-account</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>我的</v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <v-list-item link @click="commandHandler('about')">
          <v-list-item-action>
            <v-icon>mdi-help-box</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>关于</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-snackbar v-model="snackbar" :timeout="2000">
      功能后续实现哦，(ง •̀_•́)ง
      <v-btn color="blue" text @click="snackbar = false"> 关闭 </v-btn>
    </v-snackbar>
  </v-app>
</template>

<script>
// 引入主页组件Banner、书单
import selected from "@/pages/home/selected";
import boy from "@/pages/home/boy";
import gril from "@/pages/home/gril";
import original from "@/pages/home/original";

export default {
  name: "light-reading",
  components: {
    selected,
    boy,
    gril,
    original,
  },
  props: {
    source: String,
  },
  data() {
    return {
      snackbar: false,
      drawer: null,
      currentTab: "selected",
      user: this.db.get("USER"),
    };
  },
  computed: {
    currentTabComponent: function () {
      return this.currentTab;
    },
  },
  created() {
    document.title = "轻松阅读";
  },
  methods: {
    commandHandler(cmd) {
      // 退出登录
      if (cmd == "logout") {
        if (confirm("此操作将注销登录, 是否继续?")) {
          this.db.remove("USER");
          this.db.remove("TOKEN");
          this.$router.replace("/login");
        }
      } else if (cmd == "home") {
        this.drawer = false;
      } else if (cmd == "my") {
        this.snackbar = true;
      } else if (cmd == "bookshelf") {
        this.$router.push("/my-bookshelf");
      } else if (cmd == "like") {
        this.$router.push("/my-like");
      } else if (cmd == "about") {
        this.$router.push("/about");
      } else if (cmd == "login") {
        this.$router.push("/login");
      } else if (cmd == "register") {
        this.$router.push("/register");
      }
    },
    switchComponent(componentName) {
      this.currentTabComponent = componentName;
    },
  },
};
</script>

<style scoped>
.v-toolbar__content {
  height: 60px;
}
.v-application--wrap {
  background-image: url("~@/assets/backgroundImg.png");
}
</style>
