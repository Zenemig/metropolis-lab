<template>
  <div class="c-map">

  </div>
</template>

<script>
export default {
  name: 'mapView',
  props: {
    trip: {
      type: Object,
      required: true
    }
  },
  data: () => {
    return {
      routeCoordinates: [],
      routePath: null,
      map: null,
      bounds: null,
      markers: []
    }
  },
  mounted () {
    const element = document.querySelector('.c-map')
    const options = {
      zoom: 5,
      center: new google.maps.LatLng(41.3851, 2.1734),
      zoomControl: true,
      mapTypeControl: false,
      scaleControl: false,
      streetViewControl: false,
      rotateControl: true,
      fullscreenControl: true
    }

    this.map = new google.maps.Map(element, options)
  },
  watch: {
    trip: function (oldTrip, newTrip) {
      this.resetMapData()
      this.setRouteCoordinates()
      this.createMarkers()
      this.createPath()
    }
  },
  methods: {
    setRouteCoordinates: function () {
      this.routeCoordinates.push({
        lat: this.trip.origin.point._latitude,
        lng: this.trip.origin.point._longitude
      })
      this.trip.stops.map(stop => {
        if (stop.point) {
          this.routeCoordinates.push({
            lat: stop.point._latitude,
            lng: stop.point._longitude
          })
        }
      })
      this.routeCoordinates.push({
        lat: this.trip.destination.point._latitude,
        lng: this.trip.destination.point._longitude
      })
    },
    resetMapData: function () {
      this.routeCoordinates = []

      this.markers.map(mark => {
        mark.setMap(null)
      })
      this.markers = []

      if (this.routePath) {
        this.routePath.setMap(null)
      }
    },
    createMarkers: function () {
      this.bounds = new google.maps.LatLngBounds();

      this.routeCoordinates.map(coord => {
        let position = new google.maps.LatLng(coord.lat, coord.lng);
        let marker = new google.maps.Marker({
          position,
          map: this.map
        })
        this.markers.push(marker)
        this.map.fitBounds(this.bounds.extend(coord))
      })
    },
    createPath: function () {
      this.routePath = new google.maps.Polyline({
        path: this.routeCoordinates,
        geodesic: true,
        strokeColor: '#FF0000',
        strokeOpacity: 1.0,
        strokeWeight: 2
      })
      this.routePath.setMap(this.map)
    }
  }
}
</script>

<style lang="scss">
.c-map {
  width: 100%;
  height: 100%;
}
</style>
