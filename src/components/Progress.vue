<template>
  <div class="container">
    <MoreInformation/>
    <div class="my">
        <div class="info">
      <span>{{ progressBarText}} of {{yearNow}} has passed</span>
      </div>
      <div class="progress-inside"></div>
        <div class="progress" :style="{ width: progressBarWidth }"></div>
      </div>
      </div>
</template>

<script>
import MoreInformation from "./MoreInformation.vue";
export default {
  components: {
    MoreInformation,
  },
  data() {
    return {
      progressBarWidth: '0%',
      progressBarText: '',
      yearNow : '',
    };
  },
  mounted() {
    this.updateProgressBar();
    setInterval(this.updateProgressBar, 1000);
  },
  methods: {
  updateProgressBar() {
    const currentDate = new Date();
    const yearNow = currentDate.getFullYear();
    const startOfYear = new Date(currentDate.getFullYear(), 0, 1); // January 1st of the current year
    const endOfYear = new Date(currentDate.getFullYear() + 1, 0, 1); // January 1st of the next year

    const yearProgress = Math.round((currentDate - startOfYear) / (endOfYear - startOfYear) * 100);

    this.progressBarWidth = yearProgress + '%';
    this.progressBarText = yearProgress + '%';
    this.yearNow = yearNow
  },
},
};
</script>

