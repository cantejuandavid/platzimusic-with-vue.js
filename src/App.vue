<template lang="pug">
  #app
    img(src='https://cantejuandavid.github.io/platzimusic-with-vue.js/dist/logo.png')
    h1 PlatziMusic
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" :key="artist.mbid")
</template>

<script>

import Artist from './components/artist.vue'
import getArtist  from './api'
import spinner from './components/spinner.vue'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name: "Colombia", value: "colombia"},
        {name: "Argentina", value: "argentina"},
        {name: "España", value: "spain"}
      ],
      selectedCountry: 'colombia',
      loading: true
    }
  },
  components : {
    Artist: Artist,
    spinner: spinner
  },
  methods: {
    refreshArtist: function() {
      this.loading = true
      this.artists = []
      let self = this
      getArtist(this.selectedCountry)
        .then(function(artists) {    
          self.loading = false    
          self.artists = artists
        })
    }
  },
  mounted () {
    this.refreshArtist()
  },
  watch: {
    selectedCountry: function () {
      this.refreshArtist()
    }
  }
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

h1, h2
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
