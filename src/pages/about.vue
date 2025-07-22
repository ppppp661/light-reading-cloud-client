<template>
  <v-app cols="12" sm="6" offset-sm="3" align="center">
    <v-app-bar
      app
      color="#14B0ED"
      dark
      cols="12"
      src="@/assets/backgroundImg.png"
      elevation="24"
    >
      <v-btn icon @click="$router.back(-1)">
        <v-icon>mdi-arrow-left</v-icon>
      </v-btn>
      <v-toolbar-title>关于</v-toolbar-title>
    </v-app-bar>

    <!-- content -->
    <v-content style="width: 100%; text-align: center; margin-top: 15px">
      <v-container fluid>
        <v-card
          class="mx-auto elevation-12"
          max-width="800"
          color="rgba(255,255,255,0.9)"
        >
          <v-card-title
            class="justify-center text-h4 font-weight-bold primary--text"
          >
            欢迎来到轻松阅读
          </v-card-title>

          <v-divider class="mx-4"></v-divider>

          <v-card-text class="text-body-1" style="line-height: 1.8">
            <p>
              我们相信，每一本书都是一扇通往新世界的门，每一次阅读都是一次心灵的旅行。
            </p>

            <p>
              在这里，我们致力于为读者提供丰富多样的图书资源，涵盖经典文学、热门畅销、人文社科、科技新知等多个领域。无论您是寻找灵感的创作者，还是渴望知识的探索者，亦或是单纯享受阅读乐趣的书友，都能在这里找到属于您的那本书。
            </p>

            <p>
              我们的使命是让阅读更便捷、更有趣。通过精心设计的界面、个性化的推荐和活跃的读者社区，我们希望帮助您发现更多好书，分享阅读的喜悦，并与志同道合的朋友一起成长。
            </p>

            <p class="font-weight-bold text-center">
              感谢您成为光阅读的一员。让我们一起，在书海中遇见更好的自己。
            </p>
          </v-card-text>

          <v-card-actions class="justify-center pb-6">
            <v-btn
              color="primary"
              outlined
              large
              @click="$router.push('/index')"
            >
              开始阅读
              <v-icon right>mdi-book-open-variant</v-icon>
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-container>
    </v-content>
  </v-app>
</template>
<script>
export default {
  data() {
    return {
      alert: "",
      messages: [
        "为什么我天天上下班从不关电脑？想想上次关机应该是去年，关机的原因也是被迫系统更新升级。",
        "为什么大多数开发都2个显示器，我却习惯用笔记本自带的屏幕？",
        "为什么浏览器页签开了十几二十个也不打算尝试关闭一些呢？",
        "为什么我的电脑桌面没有一个应用图标？哦，大概是平时根本看不见桌面吧？",
        "为什么开的所有工具应用从不使用全屏？",
      ],
    };
  },
};
</script>
