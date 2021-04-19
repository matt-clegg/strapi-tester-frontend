<template>
  <div v-if="groups && groups.length > 0">
    <div class="group-separator">
      <div class="group-separator__dot" />
      <a v-if="canBookTopSlot" href="javascript:;" class="group-separator__text" @click="onBook">
        Book Slot
      </a>
    </div>
    <div v-for="(group, i) in groups" :key="i" class="group">
      <div v-for="(booking, bookingIndex) in group" :key="bookingIndex" class="group-item">
        <paddle-slot :booking="booking" @click="onOpenBooking(booking)" />
      </div>
      <div class="group-separator">
        <a v-if="!(i === groups.length - 1) || canBookBottomSlot" href="javascript:;" class="group-separator__text" @click="onBook">
          Book Slot
        </a>
        <div v-if="i === groups.length - 1" class="group-separator__dot group-separator__dot--bottom" />
      </div>
    </div>
  </div>
  <div v-else class="group__container">
    <div class="group-separator">
      <div class="group-separator__dot" />
      <a href="javascript:;" class="group-separator__text" @click="onBook">
        Book Slot
      </a>
      <div class="group-separator__dot group-separator__dot--bottom" />
    </div>
  </div>
</template>

<script>
import moment from 'moment'
import PaddleSlot from '@/components/paddles/PaddleSlot'

export default {
  name: 'BookingGroups',
  components: {
    PaddleSlot
  },
  props: {
    groups: {
      type: Array,
      default: null
    }
  },
  computed: {
    canBookTopSlot () {
      let result = false

      if (this.groups && this.groups.length > 0) {
        const firstGroup = this.groups[0]

        if (firstGroup.length > 0) {
          const booking = firstGroup[0]

          const firstDate = moment(booking.date)
          const previousDate = moment(firstDate).subtract(15, 'minutes')
          if (firstDate.isSame(previousDate, 'day')) {
            result = true
          }
        }
      }

      return result
    },
    canBookBottomSlot () {
      let result = false

      if (this.groups && this.groups.length > 0) {
        const lastGroup = this.groups[this.groups.length - 1]

        if (lastGroup.length > 0) {
          const booking = lastGroup[lastGroup.length - 1]

          const lastDate = moment(booking.date)
          const nextDate = moment(lastDate).add(15, 'minutes')
          if (lastDate.isSame(nextDate, 'day')) {
            result = true
          }
        }
      }

      return result
    }
  },
  methods: {
    onBook () {

    },
    onOpenBooking (booking) {
      this.$emit('open-booking', booking)
    }
  }
}
</script>

<style lang="less" scoped>
.group {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0 10px;
  margin: 5px 0;
  position: relative;

  @media ( max-width: 991px ) {
    padding: 0 5px;
  }

  &-item {
    display: flex;
    width: 100%;
    height: 60px;
    margin: 5px 0;
  }

  &__container {
    padding-bottom: 20px;
  }
}

.group-separator {
  position: relative;
  width: 100%;
  height: 60px;
  display: flex;
  justify-content: center;
  align-items: center;

  &::after {
    content: "";
    display: block;
    position: absolute;
    left: 50%;
    top: 0;
    width: 2px;
    height: 100%;
    background-color: #FDC521;
    z-index: 99;
  }

  &__dot {
    background-color: #FDC521;
    width: 8px;
    height: 8px;
    border-radius: 10px;
    position: absolute;
    left: calc(50% - 3px);
    top: -5px;

    &--bottom {
      top: inherit;
      bottom: -5px;
    }
  }

  &__text {
    background-color: #fff;
    padding: 5px 0;
    z-index: 100;
  }
}
</style>
