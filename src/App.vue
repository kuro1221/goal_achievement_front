<template>
  <div class="app">
    <h1>目標達成ダービー</h1>
    <Calendar :year="year" :month="month" :goal-data="goalData" @save-goal="saveGoal"></Calendar>
  </div>
</template>

<script>
import Calendar from './components/Calendar.vue';
import GoalInput from './components/GoalInput.vue';
import axios from 'axios';

export default {
  data() {
    return {
      year: new Date().getFullYear(),
      month: new Date().getMonth(),
      goalData: []
    }
  },
  components: {
    Calendar,
    GoalInput
  },
  mounted() {
    axios.get('http://localhost/api/achievementList')
      .then(response => {
        this.goalData = response.data;
      })
  },
}
</script>

<style>
.app {
  max-width: 800px;
  margin: 0 auto;
}
</style>
