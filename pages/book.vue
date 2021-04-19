<template>
  <div class="container">
    <h1>Book Paddle Slot</h1>
    <booking-container />
  </div>
</template>

<script>
import BookingContainer from '@/components/paddles/BookingCalandar'
import axios from 'axios'

export default {
  name: 'Book',
  components: {
    BookingContainer
  },
  data () {
    return {
      error: null,
      bookings: []
    }
  },
  async mounted () {
    try {
      const response = await axios.get('http://localhost:1337/bookings')
      this.bookings = response.data
    } catch (error) {
      this.error = error
    }
  }
}
</script>
