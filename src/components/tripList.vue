<template>
  <ul class="l-trips">
    <li v-for="(trip, index) in trips" :key="index + '-' + trip.route" class="l-trips__trip">
      <div @click="showRoute($event)" class="c-trip" :class="{'isActive': index == selectedRoutIndex}" :data-index="index">
        <header class="c-trip__header">
          <h2 class="c-trip__title">
            {{ trip.description }}
          </h2>

          <div class="c-trip__status" :class="trip.status">
            {{ trip.status }}
          </div>
        </header>

        <span class="c-trip__start-date">
          <span class="label">Start Date</span>
          {{ getDate(trip.startTime) }}
        </span>

        <span class="c-trip__start-time">
          <span class="label">Start Time</span>
          {{ getTime(trip.startTime) }}
        </span>

        <span class="c-trip__driver">
          <span class="label">Driver</span>
          {{ trip.driverName }}
        </span>
      </div>
    </li>
  </ul>
</template>

<script>
import moment from 'moment'

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
      selectedRoutIndex: ''
    }
  },
  methods: {
    getDate (rawDate) {
      return moment(rawDate).format('dddd, MMMM DD')
    },
    getTime (rawDate) {
      return moment(rawDate).format('hh:mm A')
    },
    showRoute (event) {
      this.selectedRoutIndex = event.target.closest('.c-trip').dataset.index
    }
  }
}
</script>

<style lang="scss">
.l-trips {
  margin: 0;
  padding: 0;
  list-style-type: none;
  &__trip {
    border-bottom: 1px solid #ccc;
  }
}

.c-trip {
  cursor: pointer;
  display: flex;
  flex-flow: row wrap;
  justify-content: space-between;
  align-items: flex-start;
  width: 100%;
  padding: 15px;
  background-color: #eeeeee;
  font-size: 14px;
  transition: background-color 0.3s linear;
  &:hover {
    background-color: #e4e4e4;
  }
  &.isActive {
    background-color: #cccccc;
  }
  &__header {
    flex: 1 1 100%;
    display: flex;
    flex-flow: row nowrap;
    justify-content: space-between;
    align-items: center;
  }
  &__title {
    margin: 0;
    font-size: 20px;
    font-weight: 300;
  }
  &__status {
    flex: 0 0 80px;
    display: flex;
    flex-flow: row nowrap;
    justify-content: flex-start;
    align-items: center;
    padding-left: 15px;
    font-size: 10px;
    text-transform: capitalize;
    &::before {
      content: '';
      display: block;
      margin-right: 5px;
      border-radius: 50%;
      width: 10px;
      height: 10px;
      background-color: grey;
    }
    &.ongoing {
      color: #28af53;
      &::before {
        background-color: #28af53;
      }
    }
    &.scheduled {
      color: #c4bc2b;
      &::before {
        background-color: #c4bc2b;
      }
    }
    &.cancelled {
      color: #d62424;
      &::before {
        background-color: #d62424;
      }
    }
    &.finalized {
      color: #888888;
      &::before {
        background-color: #888888;
      }
    }
  }
  &__driver {
    flex: 1 1 100%;
  }
  &__start-time {
    flex: 0 0 80px;
    padding-left: 15px;
  }
}

.label {
  display: block;
  margin-top: 15px;
  font-size: 12px;
  font-weight: 300;
}
</style>
