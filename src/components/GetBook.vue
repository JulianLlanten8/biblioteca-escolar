<template>
  <v-container>
    <v-row>
      <v-col cols="5">
        <v-img :src="bookSelected.image" class="my-3" contain height="500" />
        <div class="text-center">
          <v-btn v-if="linkDonwload" class="mx-2" dark  color="orange" :href="linkDonwload" target="_blank">
            Preview
            <v-icon dark> mdi-book-search-outline  </v-icon>
          </v-btn>
          <v-btn class="mx-2" dark  color="green" :href="bookSelected.url" target="_blank">
            Buy
            <v-icon dark> mdi-cart-arrow-down </v-icon>
          </v-btn>
        </div>
      </v-col>

      <v-col cols="7">
        <h4 class="text-h4 mb-5" v-text="bookSelected.title"></h4>
        <div>
          Year:
          <span
            class="text-subtitle-1 green--text ml-1"
            v-text="bookSelected.year"
          ></span>
        </div>
        <p>Language: {{ bookSelected.language }}</p>
        <p>Pages: {{ bookSelected.pages }}</p>
        <p class="text-body">
          Authors: <br />
          {{ bookSelected.authors }}
        </p>

        <v-rating
          :value="parseInt(bookSelected.rating)"
          color="yellow darken-3"
          background-color="grey darken-1"
          empty-icon="$ratingFull"
          half-increments
          hover
          large
          readonly
        >
        </v-rating>

        <div
          class="text-h6"
          v-if="bookSelected.subtitle"
          v-text="bookSelected.subtitle"
        ></div>
        <div class="text-subtitle-1" v-else>This is good book</div>

        <p class="text-body mt-3">Description: {{ bookSelected.desc }}</p>
        <v-chip color="red" outlined>Price: {{ bookSelected.price }}</v-chip>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  name: "GetBook",

  data: () => ({
    isbn13: 0,
    bookSelected: {},
    linkDonwload: "",
  }),

  mounted() {
    this.isbn13 = this.$route.params.selectedBook;
    if (this.isbn13) {
      this.GetBook(this.isbn13);
    } else {
      this.$router.push("/");
    }
    /* console.log(this.isbn13); */
  },

  methods: {
    async GetBook(isbn13) {
      try {
        await axios
          .get(`https://api.itbook.store/1.0/books/${isbn13}`)
          .then((res) => {
            this.bookSelected = res.data;
            this.linkDonwload = Object.values(this.bookSelected.pdf).toString();
            console.log(this.bookSelected);
          })
          .catch((err) => {
            console.error(err);
          });
      } catch (error) {}
    },
  },
};
</script>
