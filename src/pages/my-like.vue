<template>
  <v-app cols="12" align="center">
    <v-app-bar
      app
      dark
      cols="12"
      color="#14B0ED"
      src="@/assets/backgroundImg.png"
      elevation="24"
      dense
    >
      <v-btn icon @click="$router.back(-1)">
        <v-icon>mdi-arrow-left</v-icon>
      </v-btn>
      <v-toolbar-title>我的喜欢</v-toolbar-title>
    </v-app-bar>

    <!-- content -->
    <v-content style="width: 900px">
      <v-container fluid>
        <v-row no-gutters>
          <v-col
            cols="6"
            sm="6"
            v-for="book in books"
            :key="book.bookId"
            style="margin-top: 15px"
            @click="handleDetails(book.bookId)"
          >
            <v-img height="125" width="88" :src="book.imgUrl"></v-img>
            <a>{{ book.bookName }}</a>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      token: this.db.get("TOKEN"),
      books: [],
    };
  },
  created() {
    this.loadBooks();
  },
  methods: {
    // 图书详情
    handleDetails(bookId) {
      this.$router.push("/book-details/" + bookId);
    },
    // 书架图书
    loadBooks() {
      let headers = {
        token: this.token,
      };
      this.getRequest(
        "/account/like-see/get-books",
        { page: 1, limit: 100 },
        headers
      ).then((resp) => {
        if (resp.code == 200) {
          this.books = resp.data;
        }
      });
    },
  },
};
</script>
