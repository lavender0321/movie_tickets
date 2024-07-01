<template>
  <div id="app">
    <SeatSelection
      :theaters="theaters"
      :purchasedSeats="purchasedSeats"
      :reservedSeats="reservedSeats"
      :userSelectedSeats="userSelectedSeats"
      @seat-selected="handleSeatSelected"
      @submit-selection="handleSubmitSelection"
    />
  </div>
</template>

<script>
import SeatSelection from './components/SeatSelection.vue';

export default {
  name: 'App',
  components: {
    SeatSelection
  },
  data() {
    return {
      theaters: [
        {
          id: 1,
          name: 'Theater 01',
          seatData: [
            {
              row: 1,
              seats: [
                { id: 1, label: 'A1' },
                { id: 2, label: 'A2' },
                { id: 3, label: 'A3' }
              ]
            },
            {
              row: 2,
              seats: [
                { id: 4, label: 'B1' },
                { id: 5, label: 'B2' },
                { id: 6, label: 'B3' }
              ]
            }
          ]
        },
        {
          id: 2,
          name: 'Theater 02',
          seatData: [
            {
              row: 1,
              seats: [
                { id: 7, label: 'A1' },
                { id: 8, label: 'A2' },
                { id: 9, label: 'A3' }
              ]
            },
            {
              row: 2,
              seats: [
                { id: 10, label: 'B1' },
                { id: 11, label: 'B2' },
                { id: 12, label: 'B3' }
              ]
            }
          ]
        }
      ],
      purchasedSeats: [2, 5, 9],
      reservedSeats: [4, 8],
      userSelectedSeats: [6, 11]
    };
  },
  methods: {
    handleSeatSelected({ seat, theaterId }) {
      console.log('Selected seat:', seat, 'in theater:', theaterId);
    },
    handleSubmitSelection(selectedSeats) {
      console.log('Submitted selection:', selectedSeats);
    }
  }
};
</script>
