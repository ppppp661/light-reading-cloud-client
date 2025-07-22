<template>
  <v-app cols="12" sm="6" offset-sm="3" align="center">
    <v-app-bar
      app
      dark
      cols="12"
      src="@/assets/backgroundImg.png"
      color="#14B0ED"
      elevation="12"
    >
      <v-btn icon @click="$router.push('/index')">
        <v-icon>mdi-arrow-left</v-icon>
      </v-btn>
      <v-toolbar-title class="pl-0">系统登录</v-toolbar-title>
    </v-app-bar>

    <v-main class="pa-0" style="background-color: #f5f5f5">
      <v-container fluid class="fill-height">
        <v-row justify="center" align="center">
          <v-col cols="12" sm="10" md="8">
            <v-card class="elevation-6 rounded-lg" color="white" outlined>
              <v-card-title
                class="justify-center text-h4 font-weight-bold primary--text py-6"
              >
                系统登录
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
                  ></v-text-field>

                  <v-text-field
                    v-model="password"
                    :rules="passwordRules"
                    label="密码"
                    type="password"
                    outlined
                    dense
                    required
                  ></v-text-field>

                  <div class="d-flex justify-center mt-6">
                    <v-btn
                      :disabled="!valid"
                      color="primary"
                      class="mr-4 px-6"
                      @click="login"
                    >
                      登录
                    </v-btn>

                    <v-btn
                      color="light"
                      class="px-6"
                      @click="$router.push('register')"
                    >
                      点击注册账户
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
    password: "",
    passwordRules: [(v) => !!v || "请输入密码！"],
    checkbox: false,
  }),
  methods: {
    login() {
      let valid = this.$refs.form.validate();
      if (valid) {
        let formData = {
          loginName: this.loginName,
          password: this.password,
        };
        this.postRequest("/account/user/login", formData, {}).then((resp) => {
          console.log("hoee", resp);
          if (resp.code == 200) {
            this.db.save("USER", resp.data.user);
            this.db.save("TOKEN", resp.data.token);
            this.$router.replace("/index");
          } else {
            alert(resp.msg);
          }
        });
      }
    },
    reset() {
      this.$refs.form.reset();
    },
  },
};
</script>
