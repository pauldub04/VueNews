<template>
  <v-app id="inspire">
    <v-app-bar
      app
      clipped-left
    >
      <v-toolbar-title class="pageTitle">News</v-toolbar-title>

      <v-text-field
        solo-inverted
        flat
        hide-details
        label="Search by Enter hit"
        v-model="searchValue"
        clearable
        v-on:keyup.enter="loagArticles()"
      />
      <v-icon class="searchIcon" color="grey darken-4">fas fa-search</v-icon>
      <v-spacer />

      <div class="total">Всего результатов: {{total}}</div>
    </v-app-bar>

    <v-content>
      <v-container
        class="fill-height"
        fluid
      >
        <img :src="icon" alt="flag" class="flag_icon">
        <v-col class="d-flex" cols="12" sm="3">
          <v-select
            class="select"
            v-model="lang"
            :items="languages"
            :label="selectLabel"
            @change="loagArticles()"
          ></v-select>
        </v-col>
  

        <v-row
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
      color="dark"
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
    created () {
      this.$vuetify.theme.dark = true
    },
    data: () => ({
      drawer: false,
      news: [],
      lang: 'ru',
      languages: [{text: 'Россия', value:'ru'},
                  {text: 'USA', value:'us'},
                  {text: 'Great Britain', value:'gb'},
                  {text: 'Germany', value:'de'},
                  {text: 'India', value:'in'},
                  {text: 'Arab Emirates', value:'ae'},],
      total: '',
      searchValue: ''
    }),
    components: {
      ArticleTile
    },
    mounted() {
      axios.get(`https://newsapi.org/v2/top-headlines?country=${this.lang}&apiKey=d7f41a32c26b4bbfb596d58b1a54c766`)
        .then( (response) => {
          this.news = response.data.articles;
          this.total = response.data.totalResults;
          this.init();
        })
    },
    methods: {
      loagArticles() {
        axios.get(`https://newsapi.org/v2/top-headlines?country=${this.lang}&apiKey=d7f41a32c26b4bbfb596d58b1a54c766&q
=${this.searchValue}`)
        .then( (response) => {
          this.news = response.data.articles;
          this.total = response.data.totalResults;
          this.init();
        })
      },
      init() {
        for (let i = 0; i < this.news.length; i++) {
          if (this.news[i].title === '' || this.news[i].description === '')
            this.news.splice(i, 1);
        }
      }
    },
    computed: {
      icon() {
        return `/img/${this.lang}.png`;
      },
      selectLabel() {
        if (this.lang === 'ru')
          return 'Страна';
        else if (this.lang === 'de')
          return 'Land';
        else if (this.lang === 'ae')
          return 'بلد';
        else
          return 'Country';
      }
    }
  }
</script>

<style>
.flag_icon {
  width: 30px;
  margin-right: 10px;
  margin-top: -10px;
}

.total {
  text-align: right;
}

.pageTitle {
  margin-right: 50%;
}

.searchIcon {
  margin-left: 10px;
}

.searchIcon:hover {
  cursor: pointer;
}

</style>