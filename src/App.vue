<template>
  <div id="app" class="l-main-container">
    <div @click="toggleSidebar" class="a-sidebar-toggle">
      <font-awesome-icon icon="bars" />
    </div>

    <aside class="l-sidebar" :class="{'isVisible': showSidebar}">
      <header class="l-sidebar__header">
        <h2 class="l-sidebar__title">
          Available Trips
        </h2>

        <div class="l-sidebar__close" @click="toggleSidebar">
          <font-awesome-icon icon="times" />
        </div>
      </header>

      <trip-list :trips="trips" />
    </aside>

    <main class="l-map">
      <map-view />
    </main>
  </div>
</template>

<script>
import axios from 'axios'
import tripList from './components/tripList'
import mapView from './components/mapView'

export default {
  name: 'app',
  components: {
    tripList,
    mapView
  },
  data: () => {
    return {
      showSidebar: false,
      trips: []
    }
  },
  created () {
    axios
      .get('https://europe-west1-metropolis-fe-test.cloudfunctions.net/api/trips')
      .then(response => {
        this.trips = response.data
      })
      .catch(error => {
        console.log(error)
      })
  },
  methods: {
    toggleSidebar: function () {
      this.showSidebar = !this.showSidebar
    }
  }
}
</script>

<style lang="scss">
html {
  box-sizing: border-box;
}
*, *::before, *::after {
  box-sizing: inherit;
}
body {
  margin: 0;
}

.l-main-container {
  @media all and (min-width: 768px) {
    display: flex;
    flex-flow: row nowrap;
    justify-content: space-between;
    align-items: stretch;
  }
}
.l-sidebar {
  position: absolute;
  top: 0;
  left: 0;
  transform: translateX(-100%);
  width: 100%;
  height: 100vh;
  background-color: #f6f6f6;
  transition: transform 0.3s ease-in-out;
  @media all and (min-width: 450px) {
    box-shadow: 0px 2px 5px grey;
    max-width: 350px;
  }
  @media all and (min-width: 768px) {
    position: relative;
    transform: translateX(0);
    box-shadow: none;
  }
  &__header {
    display: flex;
    flex-flow: row nowrap;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 15px;
    padding: 15px;
    border-bottom: 1px solid #ccc;
    color: #333;
  }
  &__title {
    margin: 0;
    font-family: sans-serif;
    font-weight: 300;
  }
  &__close {
    flex: 0 0 30px;
    display: flex;
    flex-flow: row nowrap;
    justify-content: center;
    align-items: center;
    width: 30px;
    height: 30px;
    @media all and (min-width: 768px) {
      display: none;
    }
  }
  &.isVisible {
    transform: translateX(0);
  }
}
.l-map {
  width: 100%;
  height: 100vh;
  background-color: lightgrey;
}

.a-sidebar-toggle {
  display: flex;
  flex-flow: row nowrap;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 15px;
  left: 15px;
  box-shadow: 0px 2px 5px grey;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  background-color: white;
  color: grey;
  @media all and (min-width: 768px) {
    display: none;
  }
}
</style>
