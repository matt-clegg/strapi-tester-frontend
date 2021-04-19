<template>
  <div
    v-if="booking"
    class="paddle-slot"
    :class="{'paddle-slot--disabled': isInPast}"
    @click="$emit('click')"
  >
    <div class="paddle-slot__dates">
      <!-- <div class="paddle-slot__date">
        {{ bookingDate }}
      </div> -->
      <div class="paddle-slot__times">
        {{ startTime }}
      </div>
    </div>

    <div class="paddle-slot__content">
      <!-- <div class="paddle-slot__participants">
        Participants: {{ booking.participants }} / 6
      </div> -->
      <!-- <button
        class="button"
        :disabled="isFull"
      >
        {{ buttonText }}
      </button> -->
    </div>
  </div>
</template>

<script>
import moment from 'moment'

export default {
  name: 'PaddleSlot',
  props: {
    booking: {
      type: Object,
      default: null
    }
  },
  data () {
    return {
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
    },
    isFull () {
      return this.booking.participants >= 6
    },
    buttonText () {
      if (this.isFull) {
        return 'Full'
      }
      return 'Join'
    },
    isInPast () {
      return moment(this.booking.date).add(15, 'minutes').isBefore(moment())
    }
  },
  methods: {
    formatDate (date, format) {
      return moment(new Date(date)).format(format)
    }
  }
}
</script>

<style lang="less" scoped>
.paddle-slot {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #fff;
  flex-direction: row;
  width: 100%;
  border-radius: 5px;
  border: 1px solid #f2f2f2;
  padding: 10px;
  cursor: pointer;
  transition: transform .2s ease-out,
              color .2s ease-out;
  box-shadow:
    0 0.2px 0.4px -4px rgba(0, 0, 0, 0.017),
    0 0.5px 1px -4px rgba(0, 0, 0, 0.024),
    0 0.9px 1.9px -4px rgba(0, 0, 0, 0.03),
    0 1.6px 3.4px -4px rgba(0, 0, 0, 0.036),
    0 2.9px 6.3px -4px rgba(0, 0, 0, 0.043),
    0 7px 15px -4px rgba(0, 0, 0, 0.06)
  ;

  &:hover {
    transform: translateY(-2px);
    color: lighten(#5D6671, 10%);
  }

  &--disabled {
    cursor: not-allowed;
    background-color: rgb(243, 243, 243);
    color: lightgray;

    &:hover {
      transform: inherit;
      color: lightgray;
    }
  }

  &__date {
    font-weight: 700;
    font-size: 18px;
    margin-bottom: 5px;

  }

  &__times {
    font-size: 18px;
    font-weight: 700;
  }

  &__content {
    display: flex;
    flex-direction: row;
    align-items: center;
  }

  &__participants {
    margin-right: 20px;
  }
}
</style>
