<template>
  <gmap-map
      :center="center"
      :zoom="7"
      style="width: 100%; height: 100vh"
  >
    <gmap-polygon v-for="(polygon, index) in blackSeaPolygons" :key="index" :paths="polygon.geometry.coordinates[0]"
                  @click="mapClick($event,polygon.properties.number)">
    </gmap-polygon>
    <GmapInfoWindow
        :position="position"
        :opened="openedIndex"
        @closeclick="openedIndex = null"
    >
      <div style="display:grid;" v-html="data">
      </div>
    </GmapInfoWindow>

  </gmap-map>
</template>

<script>
/////////////////////////////////////////
// New in 0.4.0
import * as VueGoogleMaps from 'vue2-google-maps';
import Vue from 'vue';
import geojson from "../geojson";
import geodata from "../geodata";

Vue.use(VueGoogleMaps, {
  load: {
    key: 'AIzaSyAgbdtn4dWiKo27oHPmPfmD7uOaMVEODoA',
    libraries: 'places',
  },
});

export default {
  data() {
    return {
      data: "test",
      openedIndex: null,
      position: {lat: 40.0, lng: 36.0},
      center: {lat: 40.0, lng: 36.0},
      blackSeaPolygons: geojson,
      blackSeaData: geodata
    }
  },
  methods: {
    mapClick(event, index) {
      this.position = {lat: event.latLng.lat(), lng: event.latLng.lng()}
      this.openedIndex = true;
      console.log(index);
      this.data = this.blackSeaData[index]
    }
  }

}
</script>