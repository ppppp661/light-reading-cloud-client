<template>
  <v-app id="inspire" cols="12" sm="6" offset-sm="3" align="center">
    <v-app-bar
      app
      dark
      cols="12"
      color="#14B0ED"
      src="@/assets/backgroundImg.png"
      elevation="12"
    >
      <v-btn icon @click="$router.back(-1)">
        <v-icon>mdi-arrow-left</v-icon>
      </v-btn>
      <v-toolbar-title class="pl-0">账户注册</v-toolbar-title>
    </v-app-bar>

    <v-main class="pa-0" style="background-color: #f5f5f5">
      <v-container fluid class="fill-height">
        <v-row justify="center" align="center">
          <v-col cols="12" sm="10" md="8">
            <v-card class="elevation-6 rounded-lg" color="white" outlined>
              <v-card-title
                class="justify-center text-h4 font-weight-bold primary--text py-6"
              >
                账户注册
              </v-card-title>

              <v-divider class="mx-4"></v-divider>

              <v-card-text class="pa-6">
                <v-form ref="form" v-model="valid" lazy-validation>
                  <v-text-field
                    v-model="loginName"
                    :counter="10"
                    :rules="loginNameRules"
                    label="账户"
                    outlined
                    dense
                    required
                    :append-icon="loginName ? 'mdi-check-circle' : ''"
                    :success="!!loginName"
                    @click:append="() => {}"
                  >
                    <template v-slot:prepend-inner>
                      <v-icon color="primary" class="mr-2">mdi-account</v-icon>
                    </template>
                  </v-text-field>

                  <v-text-field
                    v-model="nickName"
                    :counter="10"
                    :rules="nickNameRules"
                    label="昵称"
                    outlined
                    dense
                    required
                    :append-icon="nickName ? 'mdi-check-circle' : ''"
                    :success="!!nickName"
                    @click:append="() => {}"
                  >
                    <template v-slot:prepend-inner>
                      <v-icon color="primary" class="mr-2"
                        >mdi-account-edit</v-icon
                      >
                    </template>
                  </v-text-field>

                  <v-text-field
                    v-model="userPwd"
                    :rules="userPwdRules"
                    type="password"
                    label="密码"
                    outlined
                    dense
                    required
                    :append-icon="userPwd ? 'mdi-eye' : ''"
                    @click:append="togglePasswordVisibility"
                  >
                    <template v-slot:prepend-inner>
                      <v-icon color="primary" class="mr-2">mdi-lock</v-icon>
                    </template>
                  </v-text-field>

                  <div class="d-flex justify-center mt-6">
                    <v-btn
                      :disabled="!valid"
                      color="primary"
                      class="mr-4 px-6 register-btn"
                      @click="register"
                      @mouseenter="onButtonHover"
                      @mouseleave="onButtonLeave"
                    >
                      <v-icon left>mdi-account-plus</v-icon>
                      注册
                    </v-btn>

                    <v-btn color="light" class="px-6" @click="$router.back(-1)">
                      返回
                    </v-btn>
                  </div>
                </v-form>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    loginName: "",
    loginNameRules: [
      (v) => !!v || "请输入账户！",
      (v) => (v && v.length <= 10) || "账户长度不能超过10个字符哦~",
    ],
    nickName: "",
    nickNameRules: [
      (v) => !!v || "请输入昵称！",
      (v) => (v && v.length <= 10) || "昵称长度不能超过10个字符哦~",
    ],
    userPwd: "",
    userPwdRules: [(v) => !!v || "请输入密码！"],
    checkbox: false,
    passwordVisible: false,
  }),
  methods: {
    register() {
      let valid = this.$refs.form.validate();
      if (valid) {
        let formData = {
          loginName: this.loginName,
          nickName: this.nickName,
          userPwd: this.userPwd,
        };
        this.postRequest("/account/user/register", formData, {}).then(
          (resp) => {
            console.log("hello", formData, resp);
            if (resp.code == 200) {
              alert(resp.msg);
              console.log("success");
              this.$router.replace("/login");
            } else {
              alert(resp.msg);
              console.log("fail");
            }
          }
        );
      }
    },
    reset() {
      this.$refs.form.reset();
    },
    togglePasswordVisibility() {
      // 切换密码可见性
      const passwordInput = this.$refs.form.$el.querySelector(
        'input[type="password"]'
      );
      if (passwordInput) {
        passwordInput.type = this.passwordVisible ? "password" : "text";
        this.passwordVisible = !this.passwordVisible;
      }
    },
    onButtonHover(event) {
      event.target.classList.add("pulse-animation");
    },
    onButtonLeave(event) {
      event.target.classList.remove("pulse-animation");
    },
  },
};
</script>

<style scoped>
.register-btn.pulse-animation {
  animation: pulse 1s infinite;
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
  }
}
</style>
