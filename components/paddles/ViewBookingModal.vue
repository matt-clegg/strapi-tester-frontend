<template>
  <modal v-if="show">
    <template #body>
      <div class="view-booking">
        <div class="view-booking__time">
          {{ startTime }} - {{ endTime }}
        </div>
        <div class="view-booking__date">
          {{ bookingDate }}
        </div>

        <!-- <div>
          Participants:
          <div v-for="user in booking.booking_users" :key="user.id">
            {{ user.Name }}
          </div>
        </div> -->

        <div class="view-booking__fields">
          <input type="email" placeholder="Email">
          <input type="text" placeholder="Name">
          <a type="button" class="btn btn-accent-1" @click="onJoin">
            Join
          </a>
        </div>
      </div>
    </template>
  </modal>
</template>

<script>
import moment from 'moment'
// import axios from 'axios'

export default {
  name: 'ViewBookingModal',
  props: {
    show: {
      type: Boolean,
      default: false
    },
    booking: {
      type: Object,
      default: null
    }
  },
  computed: {
    bookingDate () {
      return this.formatDate(this.booking.date, 'dddd Do MMMM')
    },
    startTime () {
      return this.formatDate(this.booking.date, 'h:mma')
    },
    endTime () {
      const time = moment(this.booking.date).add(15, 'minutes')
      return this.formatDate(time, 'h:mma')
    }
  },
  methods: {
    formatDate (date, format) {
      return moment(new Date(date)).format(format)
    },
    onJoin () {
      // axios.post('http://localhost:1337/')
    }
  }
}
</script>

<style lang="less" scoped>
.view-booking {
  min-width: 500px;

  &__time {
    font-size: 24px;
    font-weight: 700;
    margin-bottom: 5px;
  }

  &__date {
    font-size: 18px;
    margin-bottom: 20px;
  }

  &__fields {
    display: flex;
    flex-direction: column;
  }
}
</style>
