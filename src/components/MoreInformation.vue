<template>
    <div class="more-information">
      <div class="info-row">
        <strong>Days Total:</strong> {{ daysPassed }}/{{ totalDaysInYear }}
      </div>
      <div class="info-row">
        <strong>Server Date:</strong> {{ formattedServerDate }}
      </div>
      <div class="info-row">
        <strong>Year Percentage:</strong> {{ formattedYearPercentage }}
      </div>
      <div class="info-row">
        <strong>Server Time:</strong> {{ formattedServerTime }}
      </div>
      <div class="info-row">
        <strong>Server Time 12H:</strong> {{ formattedServerTime12H }}
      </div>
      <div class="info-row">
        <strong>Server Time 24H:</strong> {{ formattedServerTime24H }}
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        totalDaysInYear: 365,
        serverDate: new Date(),
        serverTime: new Date(),
      };
    },
    computed: {
      daysPassed() {
        const startDate = new Date(this.serverDate.getFullYear(), 0, 1);
        const currentDate = this.serverDate;
        const elapsedDays = Math.floor((currentDate - startDate) / (24 * 60 * 60 * 1000)) + 1;
        return elapsedDays;
      },
      yearPercentage() {
        return (this.daysPassed / this.totalDaysInYear) * 100;
      },
      formattedServerDate() {
        return this.formatDate(this.serverDate);
      },
      formattedYearPercentage() {
        return this.yearPercentage.toFixed(2) + "%";
      },
      formattedServerTime() {
        return this.formatTime(this.serverTime);
      },
      formattedServerTime12H() {
        return this.formatTime12H(this.serverTime);
      },
      formattedServerTime24H() {
        return this.formatTime24H(this.serverTime);
      },
    },
    methods: {
      formatDate(date) {
        const options = { year: "numeric", month: "2-digit", day: "2-digit" };
        return date.toLocaleDateString(undefined, options);
      },
      formatTime(time) {
        const options = { hour: "numeric", minute: "numeric", second: "numeric", hour12: true };
        return time.toLocaleTimeString(undefined, options);
      },
      formatTime12H(time) {
        const options = { hour: "2-digit", minute: "2-digit", hour12: true };
        return time.toLocaleTimeString(undefined, options);
      },
      formatTime24H(time) {
        const options = { hour: "2-digit", minute: "2-digit", hour12: false };
        return time.toLocaleTimeString(undefined, options);
      },
      // Add a method to update the data every second
      updateData() {
        setInterval(() => {
          this.serverDate = new Date(); // Update server date
          this.serverTime = new Date(); // Update server time
        }, 1000);
      },
    },
    mounted() {
      this.updateData(); // Start updating data when the component is mounted
    },
  };
  </script>
  
  <style scoped>
  .more-information {
    font-family: Arial, sans-serif;
  }
  
  .info-row {
    font-size: 18px;
    color: #666;
  }
  
  @media screen and (max-width: 768px) {
    .more-information {
      max-width: 100%;
    }
  }
  </style>
  