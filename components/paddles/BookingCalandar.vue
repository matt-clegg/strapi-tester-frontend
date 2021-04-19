<template>
  <div class="booking">
    <div class="booking__month">
      <span class="booking__month--year-month">
        {{ currentYearMonth }}
      </span>
      <span class="booking__month--month">
        {{ currentMonth }}
      </span>
    </div>
    <div class="booking-container">
      <template v-if="loading">
        <loading-spinner />
      </template>
      <template v-else>
        <div class="booking-container__column booking-container__column--slim">
          <a href="javascript:;" class="booking-container__day-heading booking-container__day-heading--week" @click="changeWeek(-1)">
            <font-awesome-icon :icon="['fas', 'chevron-left']" />
          </a>
          <a href="javascript:;" class="booking-container__day-heading booking-container__day-heading--day" @click="changeDay(-1)">
            <font-awesome-icon :icon="['fas', 'chevron-left']" />
          </a>
        </div>
        <div
          v-for="(day, index) in weekDays"
          :key="index"
          class="booking-container__column"
          :class="{'booking-container__column--highlighted': isToday(day),
                   'booking-container__column--selected': isSelected(day)}"
        >
          <div
            class="booking-container__day-heading"
            :class="{'booking-container__day-heading--highlighted': isToday(day)}"
          >
            <div class="booking-container__day-name">
              {{ formatDate(day, "ddd") }}
            </div>
            <div class="booking-container__day-date">
              {{ formatDate(day, "DD") }}
            </div>
          </div>

          <booking-groups
            :groups="getBookingGroups(day)"
            @open-booking="onOpenBooking"
          />
        </div>
        <div class="booking-container__column booking-container__column--slim">
          <a href="javascript:;" class="booking-container__day-heading booking-container__day-heading--week" @click="changeWeek(1)">
            <font-awesome-icon :icon="['fas', 'chevron-right']" />
          </a>
          <a href="javascript:;" class="booking-container__day-heading booking-container__day-heading--day" @click="changeDay(1)">
            <font-awesome-icon :icon="['fas', 'chevron-right']" />
          </a>
        </div>
      </template>
    </div>

    <view-booking-modal :show="showModal" :booking="selectedBooking" />
  </div>
</template>

<script>
import BookingGroups from '@/components/paddles/BookingGroups'
import ViewBookingModal from '@/components/paddles/ViewBookingModal'
import moment from 'moment'
import axios from 'axios'

export default {
  name: 'BookingCalandar',
  components: {
    BookingGroups,
    ViewBookingModal
  },
  data () {
    return {
      bookings: [],
      showModal: false,
      selectedBooking: null,
      loading: false,
      dayOffset: 0,
      weekOffset: 0
    }
  },
  computed: {
    selectedDate () {
      return moment().add(this.weekOffset, 'weeks').add(this.dayOffset, 'days')
    },
    currentMonth () {
      return this.formatDate(this.selectedDate, 'MMMM yyyy')
    },
    currentYearMonth () {
      let result = null

      const start = moment(this.selectedDate).startOf('isoWeek')
      const end = moment(this.selectedDate).endOf('isoWeek')

      const startMonth = start.month()
      const endMonth = end.month()

      const startYear = start.year()
      const endYear = end.year()
      const yearsMatch = startYear === endYear

      if (startMonth !== endMonth) {
        result = this.formatDate(start, 'MMMM')
        if (!yearsMatch) {
          result += ' ' + startYear + ' - '
        } else {
          result += ' - '
        }
        result += this.formatDate(end, 'MMMM') + ' ' + endYear
      } else {
        result = this.formatDate(this.selectedDate, 'MMMM yyyy')
      }

      return result
    },
    weekDays () {
      const weekStart = this.selectedDate.clone().startOf('isoWeek')

      const days = []
      for (let i = 0; i < 7; i++) {
        days.push(moment(weekStart).add(i, 'days'))
      }

      return days
    }
  },
  async mounted () {
    try {
      this.loading = true
      const response = await axios.get('http://localhost:1337/bookings')
      window.setTimeout(() => {
        this.bookings = response.data
        this.loading = false
      }, 250)
    } catch (error) {
      this.error = error
    }
  },
  methods: {
    onOpenBooking (booking) {
      this.selectedBooking = booking
      this.showModal = true
    },
    formatDate (date, format) {
      return moment(new Date(date)).format(format)
    },
    changeWeek (value) {
      this.weekOffset += value
    },
    changeDay (value) {
      this.dayOffset += value
    },
    isToday (day) {
      return moment().isSame(moment(day), 'day')
    },
    isSelected (day) {
      return this.selectedDate.isSame(moment(day), 'day')
    },
    getBookingGroups (day) {
      const bookings = this.getBookingsForDay(day)

      const groups = []

      if (bookings && bookings.length > 0) {
        let currentGroup = []
        let currentInterval = -1
        for (let i = 0; i < bookings.length; i++) {
          const booking = bookings[i]
          const interval = Math.floor(+(new Date(booking.date)) / (1000 * 60 * 15))

          const diff = currentInterval === -1 ? 1 : Math.abs(interval - currentInterval)
          if (diff !== 1) {
            groups.push(currentGroup)
            currentGroup = []
          }

          currentGroup.push(booking)
          currentInterval = interval
        }

        if (currentGroup && currentGroup.length > 0) {
          groups.push(currentGroup)
        }
      }
      return groups
    },
    getBookingsForDay (day) {
      return this.bookings.filter(b => moment(b.date).isSame(day, 'day'))
        .sort((a, b) => new Date(a.date) - new Date(b.date))
    }
  }
}
</script>

<style lang="less" scoped>
.booking {
  &__month {
    font-size: 22px;
    margin-bottom: 20px;

    &--year-month {
      @media (max-width: 846px){
        display: none;
      }
    }

    &--month {
      @media ( min-width: 846px ){
        display: none;
      }
    }
  }
}

.booking-container{
  display: flex;
  justify-content: space-between;

  &__column {
    display: flex;
    flex-direction: column;
    width: 100%;
    min-width: 69px;
    overflow: hidden;

    @media ( max-width: 846px ) {
      display: none;
    }

    &--slim {
      min-width: 50px;
      width: 50px;

      @media ( max-width: 846px ) {
        min-width: 40px;
        width: 40px;
      }

      @media ( max-width: 846px ){
        display: block;
      }
    }

    &--selected {
      @media (max-width: 846px ){
        display: block;
      }
    }
  }

  &__day{
    &-heading {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 48px;
      padding: 30px 0;
      margin-bottom: 30px;
      border-top: 1px solid lightgray;
      border-bottom: 1px solid lightgray;

      &--highlighted {
        color: darken(#FDC521, 10%);
      }

      &--day {
        display: none;
        @media (max-width: 846px){
          display: flex;
        }
      }

      &--week {
        @media (max-width: 846px){
          display: none;
        }
      }
    }

    &-name {
      width: 100%;
      text-transform: uppercase;
      font-size: 14px;
      font-weight: 700;
      text-align: center;
    }

    &-date {
      width: 100%;
      font-size: 28px;
      font-weight: 700;
      text-align: center;
    }
  }
}
</style>
