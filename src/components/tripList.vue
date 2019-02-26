<template>
  <ul class="c-trips">
    <li class="c-trips__trip" v-for="trip in trips" :key="trip.route">

    </li>
  </ul>
</template>

<script>
export default {
  name: 'tripList',
  props: {
    trips: {
      type: Array,
      required: true
    }
  },
  data: () => {
    return {

    }
  },
  methods: {

  }
}
</script>

<style lang="scss">

</style>
