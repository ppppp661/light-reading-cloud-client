<template>
  <v-app cols="12" align="center">
    <v-app-bar
      app
      dark
      cols="12"
      src="@/assets/backgroundImg.png"
      color="#14B0ED"
      elevation="24"
      dense
    >
      <v-btn icon @click="$router.back(-1)">
        <v-icon>mdi-arrow-left</v-icon>
      </v-btn>
      <v-toolbar-title>我的书架</v-toolbar-title>
    </v-app-bar>

    <!-- content -->
    <v-content style="width: 900px">
      <v-container fluid>
        <v-row no-gutters>
          <v-col
            cols="4"
            sm="4"
            v-for="book in books"
            :key="book.bookId"
            style="margin-top: 15px"
            @click="readBook(book.bookId, book.lastChapterId)"
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
    // 阅读图书
    readBook(bookId, chapterId) {
      this.$router.push("/book-read/" + bookId + "/" + chapterId + "/1");
    },
    // 书架图书
    loadBooks() {
      let headers = {
        token: this.token,
      };
      this.getRequest("/account/bookshelf/get-books", {}, headers).then(
        (resp) => {
          if (resp.code == 200) {
            this.books = resp.data;
          }
        }
      );
    },
  },
};
</script>
