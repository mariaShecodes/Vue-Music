<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>PlatziMusic</h1>
    <select v-model="selectedCountry">
      <option v-for="country in countries" :value="country.value">{{ country.name }}</option>
    </select>
    <spinner v-show="loading"></spinner>
    <ul>
      <artist
        v-for="artist in artists"
        v-bind:artist="artist" v-bind:key="artist.mbid">
      </artist>
    </ul>
  </div>
</template>

<script>
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtists from './api'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        { name: 'Argentina', value: 'argentina'},
        { name: 'Colombia', value: 'colombia'},
        { name: 'España', value: 'spain'},
        { name: 'Portugal', value: 'portugal'},
        { name: 'Italia', value: 'italy'}
      ],
      selectedCountry: 'argentina',
      loading: true
    }
  },
  components: {
    Artist: Artist,   // = Artist,
    Spinner
  },
  methods: {
    refreshArtists() {
      const _this= this
      this.loading = true
      this.artists = []
      getArtists(this.selectedCountry)
        .then(function (artists) {
          _this.loading = false
          _this.artists = artists
        })
    }
  },
  mounted() {
    this.refreshArtists()          

  },
  watch: {                      
    selectedCountry () {
      this.refreshArtists()
    }
  }

}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

a {
  color: #42b983;
  text-decoration-line: none;
}
</style>
