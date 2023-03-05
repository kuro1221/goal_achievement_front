<template>
    <div class="calendar-container">
        <div class="calendar-grid">
            <!-- <div class="calendar-cell header">日</div>
            <div class="calendar-cell header">月</div>
            <div class="calendar-cell header">火</div>
            <div class="calendar-cell header">水</div>
            <div class="calendar-cell header">木</div>
            <div class="calendar-cell header">金</div>
            <div class="calendar-cell header">土</div> -->
            <div v-for="day in daysInMonth" :key="day" class="calendar-cell">
                {{ day }}
                <div v-for="goal in goalData.data">
                    <select v-model="selected">
                        <option :value="1" v-bind:selected="goal.status == 1">○</option>
                        <option :value="2" v-bind:selected="goal.status == 2">休</option>
                        <option :value="3" v-bind:selected="goal.status == 3">×</option>
                    </select>
                    {{ goal.status }}
                </div>
            </div>
            <!-- <div v-for="goal in goals" :key="goal.goal_id" class="calendar-cell">{{ goal.goal_name }}</div> -->
        </div>
    </div>
</template>
  
<script>
export default {
    props: ['year', 'month', 'goalData'],
    data() {
        return {
            goals: [{
                goal_id: 1,
                goal_name: "瞑想",
                user_id: 1
            },
            {
                goal_id: 2,
                goal_name: "筋トレ",
                user_id: 2
            }]
        };
    },
    computed: {
        daysInMonth() {
            let lastDay = new Date(this.year, this.month + 1, 0).getDate();
            console.log(lastDay);
            return Array.from({ length: lastDay }, (_, i) => i + 1);
        }
    }
};
</script>
  
<style scoped>
.calendar-container {
    overflow-x: scroll;
    height: 200px;
}

.calendar-grid {
    display: flex;
    flex-wrap: nowrap;
    grid-template-columns: repeat(31, 1fr);
}

.calendar-cell {
    padding: 10px;
    font-size: 16px;
    text-align: center;
    box-shadow: 0px 0px 4px rgba(0, 0, 0, 0.1);
    background-color: #fff;
}

.header {
    background-color: #f5f5f5;
    font-weight: bold;
}
</style>
  