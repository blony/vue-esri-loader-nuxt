<template>
  <section class="container">
    <div>
    
      <h1 class="title">
        vue-map
      </h1>
      <div id="viewDiv"></div>
      <div class="links">
        <a href="https://nuxtjs.org/" target="_blank" class="button--green">Documentation</a>
        <a href="https://github.com/nuxt/nuxt.js" target="_blank" class="button--grey">GitHub</a>
      </div>
       <nuxt-link to="/" class="button--green">
      Home page
      </nuxt-link>
      <nuxt-link to="/about" class="button--green">
      About page
      </nuxt-link>
    </div>
  </section>
</template>

<script>
import * as esriLoader from 'esri-loader'
export default {
  mounted () {
    console.log('map:mounted')
    const createMap = () => {
      // first, we use Dojo's loader to require the map class
      esriLoader.loadModules(['esri/Map', 'esri/views/SceneView', 'esri/layers/TileLayer'])
        .then(([Map, SceneView, TileLayer]) => {
          // then we load a web map from an id
          let map = new Map('map')
          let layer = new TileLayer({
            url: 'http://map.geoq.cn/arcgis/rest/services/ChinaOnlineCommunity/MapServer'
          })
          map.layers.add(layer)
          // and we show that map in a container w/ id #viewDiv
          const view = new SceneView({
            map: map,
            container: 'viewDiv'
          })
        })
        .catch(err => {
          // handle any errors
          console.error(err)
        })
    }
    if (!esriLoader.isLoaded()) {
      esriLoader.bootstrap((err) => {
        if (err) {
          console.log(err)
        }
        createMap()
      }, {
        url: 'https://js.arcgis.com/4.5/'
      })
    } else {
      createMap()
    }
  }
}
</script>
<style>
@import url('https://js.arcgis.com/4.5/esri/css/main.css') ;

#viewDiv {
    height: 500px;
    width: 100%;
}
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
  display: none;
}
</style>
