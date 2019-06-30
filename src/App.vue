<template lang="pug">
  #app
    img(src="./assets/logo.png")
    h1 {{ msg }}

    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{ country.name }}

    cSpinner(v-show="loading")

    ul
      cArtist( v-for="artist in aArtists" v-bind:artist="artist" v-bind:key="artist.mbid")
</template>

<script>
import cArtist from './components/Artist.vue'
import cSpinner from './components/Spinner.vue'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      msg: 'GiovannyGonzalez.com',
      aArtists:[],
      countries:[
        { name: 'Argentina', value: 'argentina'},
        { name: 'Colombia', value: 'colombia'},
      ],
      selectedCountry: 'argentina',
      loading: true
    }
  },
  components: {
    cArtist,
    cSpinner
  },
  methods: {
    refreshArtists() {
      const self = this
      this.loading = true
      getArtists(this.selectedCountry)
        .then(function(artists) {
          self.loading = false
          self.aArtists = artists
        })
    }
  },
  mounted: function () {
    this.refreshArtists()
  },
  watch: {
    selectedCountry() {
      this.refreshArtists()
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
