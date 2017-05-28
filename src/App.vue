<template lang="pug">
#app
  img(src='./assets/logo.png')
  h1 PlatziMusic
  select(v-model="selectedCountry")
    option(v-for="country in countries" :value="country.value") {{country.name}}
  spinner(v-show="loading")
  ul(v-show="!loading")
    artist(v-for="artist in artists" :artist="artist" :key="artist.mbid") {{artist.name}}
</template>

<script>
import getArtists from './api'
import artist from './components/artist.vue'
import spinner from './components/spinner.vue'

export default {
  name: 'app',
  data () {
    return {
      artists:[],
      countries: [
        {name:'Argentina', value:"argentina"},
        {name:'Colombia', value:"colombia"},
        {name:'España', value:"spain"},
        {name:'Kenia', value:"kenia"}
      ],
      selectedCountry: 'Argentina',
      loading: true,
    }
  },
  components:{
    artist, spinner
  },
  methods:{
    refreshArtist(){
      const self = this;
      this.loading = true;
      getArtists(this.selectedCountry).then(function (artists){
        setTimeout(function(){true}, 1000)
        self.loading = false;
        self.artists = artists;
      })
    }
  },
  mounted(){
    this.refreshArtist();
  },
  watch:{
    selectedCountry: function(){
      this.refreshArtist();
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
    color red
</style>
