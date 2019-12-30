<template>
  <v-app id="inspire">
    <v-navigation-drawer
      v-model="drawer"
      app
    >
      <v-list dense>
        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-home</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Home</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        <v-list-item link>
          <v-list-item-action>
            <v-icon>mdi-contact-mail</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>Contact</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar
      app
      color="green darken-3"
      dark
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-toolbar-title>News</v-toolbar-title>
    </v-app-bar>

    <v-content>
      <v-container
        class="fill-height"
        fluid
      >
        <v-select
          v-model="lang"
          :items="languages"
          label="Язык"
          @change="loagArticles()"
        ></v-select>

        <v-row
          align="center"
          justify="center"
        >
            <!--###############################################-->
            <ArticleTile v-for="el in news"
              :key="el.id"
              :author="el.author" 
              :title="el.title"
              :desc="el.description"
              :url="el.url"
              :img="el.urlToImage"
            >
            </ArticleTile>
        </v-row>
      </v-container>
    </v-content>
    <v-footer
      color="indigo"
      app
    >
      <span class="white--text">&copy; 2019</span>
    </v-footer>
  </v-app>
</template>

<script>
  import ArticleTile from './components/ArticleTile.vue'
  import axios from 'axios'

  export default {
    props: {
      source: String,
    },
    data: () => ({
      drawer: false,
      news: [],
      lang: 'ru',
      languages: [{text: 'Русский', value:'ru'}, {text: 'English', value:'us'}],
    }),
    components: {
      ArticleTile
    },
    mounted() {
      axios.get(`https://newsapi.org/v2/top-headlines?country=${this.lang}&apiKey=d7f41a32c26b4bbfb596d58b1a54c766`)
        .then( (response) => {
          this.news = response.data.articles;
          this.init();
        })
    },
    methods: {
      loagArticles() {
        axios.get(`https://newsapi.org/v2/top-headlines?country=${this.lang}&apiKey=d7f41a32c26b4bbfb596d58b1a54c766`)
        .then( (response) => {
          this.news = response.data.articles;
          this.init();
        })
      },
      init() {
        for (let i = 0; i < this.news.length; i++) {
          if(this.news[i].title === null || this.news[i].description === null)
            this.news.splice(i, 1);
        }
      }
    },
    computed: {
    }
  }
</script>