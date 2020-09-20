<template>
  <div id="app">
<!-- ========= App bar =========== -->

      <v-app-bar app>

        <v-app id="inspire">

            <v-autocomplete
                    v-model="select"
                    :loading="loading"
                    :items="items"
                    :search-input.sync="search"
                    cache-items
                    class="mx-4"
                    flat
                    hide-no-data
                    hide-details
                    label="Word in the title of films"
                    solo-inverted
            ></v-autocomplete>
        </v-app>
      </v-app-bar>

<!-- ========= Cards ============= -->
    <div>
      <v-card
              class="mx-auto"
      >

          <v-row dense>
            <v-col v-for="(course, index) in courses" :key="index"
                   :cols="12" cm="6" md="4" lg="3" xl="3">
              <v-card >
                <a v-bind:href="otherSite(course.imdbID)">
                  <img :src="course.Poster" class="imgInCard">
                  <div class="textInCard">
                    <p class="titleInCard"> {{course.Title}} </p>
                    <p>Year: {{course.Year}}</p>
                    <p>ID: {{course.imdbID}}</p>
                  </div>
                </a>
              </v-card>
            </v-col>
          </v-row>

      </v-card>
    </div>
  </div>
</template>

<script>
    import Vuetify from 'vuetify';
    import axios from 'axios';

    export default {
        vuetify: new Vuetify(),
        name: 'App',

        data() {
            return {
                courses: null,
                loading: false,
                items: [],
                search: null,
                select: null,
                states: [
                    'word', 'letter', 'number', 'person', 'pen', 'class', 'people', 'sound', 'water', 'side', 'place', 'man', 'woman', 'boy', 'Hawaii', 'girl', 'year', 'day', 'week', 'month', 'name', 'line', 'air', 'land', 'home', 'hand', 'house', 'picture', 'Mississippi', 'animal', 'mother', 'father', 'brother', 'sister', 'new', 'world', 'New York', 'head', 'north', 'page', 'country', 'question', 'answer', 'school', 'plant', 'Puerto Rico', 'sun', 'state', 'city', 'Tennessee', 'Texas', 'south', 'east', 'child', 'west', 'Washington', 'vacation', 'Wisconsin', 'Wyoming', 'valley',
                ],
            }
        },
        watch: {
            search(val) {
                val && val !== this.select && this.querySelections(val)
                if (val == this.select) {
                    axios
                        .get('http://www.omdbapi.com/?apikey=e9a2816f&' + 's=' + val)
                        .then(response => this.courses = response.data.Search)
                }
            },
        },
        methods: {
            querySelections(v) {
                this.loading = true
                // Simulated ajax query
                setTimeout(() => {
                    this.items = this.states.filter(e => {
                        return (e || '').toLowerCase().indexOf((v || '').toLowerCase()) > -1
                    })
                    this.loading = false
                }, 500)
            },
            otherSite(id) {
               return 'https://www.imdb.com/title/' + id + '/';
            }
        },
        mounted() {
            axios
                .get('http://www.omdbapi.com/?apikey=e9a2816f&s="Love"')
                .then(response => this.courses = response.data.Search)
        }
    }

</script>

<style>
  .v-application--wrap {
    min-height: 5vh !important;
  }
  .v-toolbar__content, .v-toolbar__extension {
    display: block !important;
  }

  .titleInCard {
    height: 50px;
    font-weight: bolder;
    font-size: large;
  }

  .textInCard {
    padding: 10px;
  }

  .imgInCard {
    width: 100%;
    height: 60vh;
  }

  a {
    text-decoration: none;
    color: black;
  }


</style>