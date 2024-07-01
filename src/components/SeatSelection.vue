<template>
  <div class="seat-selection">
    <van-tabs v-model="activeTab">
      <van-tab v-for="theater in theaters" :key="theater.id" :title="theater.name">
        <div class="seat-map">
          <div v-for="row in theater.seatData" :key="row.row" class="seat-row">
            <div
              v-for="seat in row.seats"
              :key="seat.id"
              :class="['seat', seat.status]"
              @click="selectSeat(seat, theater.id)"
            >
              {{ seat.label }}
            </div>
          </div>
        </div>
      </van-tab>
    </van-tabs>
    <van-button type="primary" @click="submitSelection">Continue</van-button>
  </div>
</template>

<script>
export default {
  name: 'SeatSelection',
  props: {
    theaters: {
      type: Array,
      required: true
    },
    purchasedSeats: {
      type: Array,
      default: () => []
    },
    reservedSeats: {
      type: Array,
      default: () => []
    },
    userSelectedSeats: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      activeTab: 0
    };
  },
  methods: {
    selectSeat(seat, theaterId) {
      if (seat.status === 'purchased' || seat.status === 'reserved' || seat.status === 'userReserved') {
        return;
      }
      seat.status = seat.status === 'available' ? 'selected' : 'available';
      this.$emit('seat-selected', { seat, theaterId });
    },
    submitSelection() {
      const selectedSeats = [];
      this.theaters.forEach(theater => {
        theater.seatData.forEach(row => {
          row.seats.forEach(seat => {
            if (seat.status === 'selected') {
              selectedSeats.push({ theaterId: theater.id, seat });
            }
          });
        });
      });
      this.$emit('submit-selection', selectedSeats);
    }
  },
  mounted() {
    this.theaters.forEach(theater => {
      theater.seatData.forEach(row => {
        row.seats.forEach(seat => {
          if (this.purchasedSeats.includes(seat.id)) {
            seat.status = 'purchased';
          } else if (this.reservedSeats.includes(seat.id)) {
            seat.status = 'reserved';
          } else if (this.userSelectedSeats.includes(seat.id)) {
            seat.status = 'userReserved';
          } else {
            seat.status = 'available';
          }
        });
      });
    });
  }
};
</script>

<style>
.seat-selection {
  padding: 20px;
}
.seat-map {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.seat-row {
  display: flex;
  margin-bottom: 10px;
}
.seat {
  width: 30px;
  height: 30px;
  margin-right: 5px;
  text-align: center;
  line-height: 30px;
  cursor: pointer;
}
.seat.available {
  background-color: green;
}
.seat.selected {
  background-color: blue;
}
.seat.purchased {
  background-color: red;
  cursor: not-allowed;
}
.seat.reserved {
  background-color: orange;
  cursor: not-allowed;
}
.seat.userReserved {
  background-color: purple;
  cursor: not-allowed;
}
</style>
