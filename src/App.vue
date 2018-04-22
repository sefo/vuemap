<template>
  <div id="app">
    <img src="./assets/logo.png">
    <VueMap msg="test"/>
    <mapbox
      access-token="undefined"
      :map-options="{
        style: {
          version: 8,
          sources: {
            osm2vectortiles: {
              type: 'vector',
              url: 'https://osm2vectortiles.tileserver.com/v2.json'
            }
          },
          layers: [
            {
              id: 'background',
              type: 'background',
              paint: {
                'background-color': '#41afa5'
              }
            },
            {
              id: 'water',
              type: 'fill',
              source: 'osm2vectortiles',
              'source-layer': 'water',
              filter: ['==', '$type', 'Polygon'],
              paint: {
                  'fill-color': '#3887be'
              }
            }
          ]
        },
        center: [-96, 37.8],
        zoom: 3
      }"
      @map-load="mapLoaded"
      @map-click="mapClicked"
    >
    </mapbox>
  </div>
</template>

<script>
import VueMap from './components/VueMap.vue'
import Mapbox from 'mapbox-gl-vue'

export default {
  name: 'app',
  components: {
    VueMap,
    Mapbox
  },
  methods: {
    mapLoaded(map) {
      map.addLayer({
        'id': 'points',
        'type': 'symbol',
        'source': {
          'type': 'geojson',
          'data': {
            'type': 'FeatureCollection',
            'features': [{
              'type': 'Feature',
              'geometry': {
                'type': 'Point',
                'coordinates': [-77.03238901390978, 38.913188059745586]
              },
              'properties': {
                'title': 'Mapbox DC',
                'icon': 'monument'
              }
            }, {
              'type': 'Feature',
              'geometry': {
                'type': 'Point',
                'coordinates': [-122.414, 37.776]
              },
              'properties': {
                'title': 'Mapbox SF',
                'icon': 'harbor'
              }
            }]
          }
        },
        'layout': {
          'icon-image': '{icon}-15',
          'text-field': '{title}',
          'text-font': ['Open Sans Semibold', 'Arial Unicode MS Bold'],
          'text-offset': [0, 0.6],
          'text-anchor': 'top'
        }
      });
    },
    mapClicked(map, e) {
      alert('Map Clicked!');
    },
  }
}
</script>

<style lang="scss">
  #map {
    width: 100%;
    height: 500px;
  }
</style>
