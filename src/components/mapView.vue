<template>
  <div class="c-map">

  </div>
</template>

<script>
export default {
  name: 'mapView',
  data: () => {
    return {

    }
  },
  mounted: function () {
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
    const map = new google.maps.Map(element, options)
  },
  methods: {

  }
}
</script>

<style lang="scss">
.c-map {
  width: 100%;
  height: 100%;
}
</style>
