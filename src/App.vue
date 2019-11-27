<template>
  <div id="app">
    <h1>lgtm.clone</h1>
    <div v-for="image in images" v-bind:key="image.id">
      <img :src="image.images.fixed_height.url">
      <pre>
        ![]({{ image.images.fixed_height.url }})
      </pre>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  },
  data () {
    return {
      images: [],
      limit: 8,
      // what giphy has to offer
      maxResults: 50
    }
  },
  computed: {
    offset () {
      return this.page * this.limit
    },
    page () {
      return this.getRandomInt(this.maxResults / this.limit)
    }
  },
  methods: {
    getRandomInt (max) {
      return Math.floor(Math.random() * Math.floor(max))
    },
    shuffle (array) {
      return array.sort(() => Math.random() - 0.5)
    }
  },
  mounted () {
    const api_key = '0aqJyd4OFUcq2tZ3tgAvDSC02DnEBgds'
    const q =  'lgtm'
    this.$http.get(`https://api.giphy.com/v1/gifs/search?api_key=${api_key}&q=${q}&offset=${this.offset}&limit=${this.limit}`).then(res => {
      return res.json()
    }).then(res => {
      this.images = this.shuffle(res.data)
    })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
