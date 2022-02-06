<template>
  <v-container fluid>
    <div style="background: #444444; height: 30px"></div>
    <v-toolbar flat color="grey lighten-2" class="mb-5 d-flex justify-center">
      <v-toolbar-title>Biblioteca Escolar </v-toolbar-title>
    </v-toolbar>

    <v-data-table
      :headers="headers"
      :search="search"
      :items="books"
      :items-per-page="10"
      :loading="load"
      @click:row="moreInfo"
      class="elevation-1"
    >
      <template v-slot:[`item.actions`]="{ item }">
        <v-btn
          icon
          color="indigo"
          small
          class="mr-2"
          @click.stop.prevent="options(item)"
        >
          <v-icon>mdi-dots-vertical</v-icon>
        </v-btn>
      </template>

      <template v-slot:top>
        <v-text-field
          v-model="search"
          placeholder="search"
          append-icon="mdi-magnify"
          clearable
          filled
          rounded
          width="200"
        ></v-text-field>
      </template>
    </v-data-table>

    <v-dialog v-model="dialog" width="400">
      <v-card>
        <v-img
          height="350"
          transition="scale-transition"
          :alt="book.title"
          :src="book.image"
        ></v-img>
        <v-card-title class="text-h5 mb-3" v-text="book.title"></v-card-title>
        <v-card-subtitle
          class="light-green--text subtitle-1"
          v-text="book.price"
        ></v-card-subtitle>
        <v-card-text v-if="book.subtitle" v-text="book.subtitle"></v-card-text>
        <v-card-text v-else>This book has no subtitle ❌</v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="dialog = false"> close </v-btn>
          <v-btn
            color="success"
            :to="{ name: 'GetBook', params: { selectedBook: book.isbn13 } }"
            >Go to Book</v-btn
          >
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  name: "library",

  data() {
    return {
      books: [],
      load: true,
      search: "",
      dialog: false,
      book: [],
      headers: [
        {
          text: "Tittle",
          align: "start",
          value: "title",
        },
        { text: "Subtitle", value: "subtitle" },
        { text: "Isbn13", value: "isbn13" },
        { text: "Price", value: "price" },
        { text: "Url", value: "url" },
        { text: "Options", value: "actions", sortable: false },
      ],
    };
  },
  mounted() {
    axios
      .get("https://api.itbook.store/1.0/new")
      .then((res) => {
        const library = res.data.books;
        this.books = library;
        this.load = false;
        console.log(this.books);
      })
      .catch((err) => {
        console.error(err);
      });
  },

  methods: {
    moreInfo(bookInfo) {
      this.book = bookInfo;
      this.dialog = true;
    },

    options(item) {
      console.log(item);
    },
  },
};
</script>
