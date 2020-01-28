<template>
  <div class="hello">
    <div id="floating-panel">
      <input @click="clearMarkers()" type="button" value="Hide Markers" />
      <input @click="showMarkers()" type="button" value="Show All Markers" />
      <input @click="deleteMarkers()" type="button" value="Delete Markers" />
    </div>
    <div id="map" ref="map"></div>
  </div>
</template>

<script>
import gmapsInit from "../utils/maps";

export default {
  name: "HelloWorld",
  data() {
    return {
      google: undefined,
      map: undefined,
      markers: []
    };
  },
  async mounted() {
    try {
      this.google = await gmapsInit();
      this.initMap();
    } catch (error) {
      console.error(error);
    }
  },
  methods: {
    initMap() {
      const haightAshbury = { lat: 52.379189, lng: 4.899431 };

      this.map = new this.google.maps.Map(this.$refs.map, {
        zoom: 12,
        center: haightAshbury,
        mapTypeId: "terrain"
      });

      this.map.addListener("click", function(event) {
        this.addMarker(event.latLng);
      });

      this.addMarker(haightAshbury);
    },
    addMarker(location) {
      this.markers.push(
        new this.google.maps.Marker({
          position: location,
          map: this.map
        })
      );
    },
    setMapOnAll(map) {
      for (let i = 0; i < this.markers.length; i++) {
        this.markers[i].setMap(map);
      }
    },
    clearMarkers() {
      this.setMapOnAll(null);
    },
    showMarkers() {
      this.setMapOnAll(this.map);
    },
    deleteMarkers() {
      this.clearMarkers();
      this.markers = [];
    }
  }
};
</script>

<style lang="scss" scoped>
#map {
  display: block;
  height: 500px;
  width: 100%;
}
</style>
