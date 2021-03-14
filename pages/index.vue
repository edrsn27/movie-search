<template>
  <v-container>
    <v-row>
      <v-col cols="12" sm="2">
        <v-sheet rounded="lg" min-height="268">
          <v-card-text>
            <v-text-field
              v-model="query"
              :counter="50"
              label="Search movie"
              required
            ></v-text-field>
          </v-card-text>
          <v-card-text
            v-for="(item, index) in movies"
            :key="index"
            @click.stop="selectedMovie = item"
          >
            <v-row align="center" class="mx-0">
              <v-rating
                :value="parseFloat(item.imdbRating)"
                color="amber"
                dense
                half-increments
                readonly
                size="14"
                length="10"
              ></v-rating>

              <div class="grey--text ml-4">
                {{ item.imdbRating }} ({{ item.imdbVotes }})
              </div>
            </v-row>

            <div class="my-4 subtitle-1">
              <a>{{ item.Title }}</a>
            </div>

            <div>
              {{ item.Genre }}
            </div>
            <hr />
          </v-card-text>
        </v-sheet>
      </v-col>

      <v-col cols="12" sm="8">
        <v-sheet min-height="70vh" rounded="lg">
          <v-card class="elevation-0" v-if="selectedMovie">
            <v-card-title class="headline">
              {{ selectedMovie.Title }}
            </v-card-title>
            <v-card-text>
              <v-img
                :lazy-src="
                  require(`~/assets/images/${selectedMovie.imdbID}.jpg`)
                "
                max-height="500"
                max-width="500"
                :src="require(`~/assets/images/${selectedMovie.imdbID}.jpg`)"
              ></v-img>

              <p>
                {{ selectedMovie.Plot }}
              </p>

              <v-rating
                :value="parseFloat(selectedMovie.imdbRating)"
                color="amber"
                dense
                half-increments
                readonly
                size="14"
                length="10"
              ></v-rating>

              <div class="grey--text ml-4">
                {{ selectedMovie.imdbRating }} ({{ selectedMovie.imdbVotes }})
              </div>

              <p>Runtime : {{ selectedMovie.Runtime }}</p>
              <p>Genre: {{ selectedMovie.Genre }}</p>
              <p>Director : {{ selectedMovie.Director }}</p>
              <p>Writer : {{ selectedMovie.Writer }}</p>
              <p>Actors : {{ selectedMovie.Actors }}</p>
            </v-card-text>
          </v-card>
        </v-sheet>
      </v-col>

      <v-col cols="12" sm="2">
        <v-sheet rounded="lg" min-height="268">
          <h1>Top 5 movies</h1>
          <v-card-text
            v-for="(item, index) in topTenMovies"
            :key="index"
            @click.stop="selectedMovie = item"
          >
            <v-row align="center" class="mx-0">
              <v-rating
                :value="parseFloat(item.imdbRating)"
                color="amber"
                dense
                half-increments
                readonly
                size="14"
                length="10"
              ></v-rating>

              <div class="grey--text ml-4">
                {{ item.imdbRating }} ({{ item.imdbVotes }})
              </div>
            </v-row>

            <div class="my-4 subtitle-1">
              <a>{{ item.Title }}</a>
            </div>

            <div>
              {{ item.Genre }}
            </div>
            <hr />
          </v-card-text>
        </v-sheet>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import Logo from "~/components/Logo.vue";
import VuetifyLogo from "~/components/VuetifyLogo.vue";
export default {
  components: {
    Logo,
    VuetifyLogo
  },
  data: () => ({
    loading: false,
    selection: 1,
    query: "",
    movies: [],
    topTenMovies: [],
    selectedMovie: null
  }),
  watch: {
    async query(query) {
      if (!query) {
        this.movies = [];
        return;
      }

      this.movies = await this.$content("movies")
        .sortBy("createdAt", "asc")
        .limit(5)
        .search(query)
        .fetch();
      console.log(this.movies);
    }
  },
  async asyncData({ $content }) {
    const topTenMovies = await $content("movies")
      .sortBy("imdbRating", "desc")
      .limit(5)
      .fetch();

    return {
      topTenMovies,
      selectedMovie: topTenMovies[0]
    };
  },
  methods: {
    reserve() {
      this.loading = true;
      setTimeout(() => (this.loading = false), 2000);
    }
  }
};
</script>
