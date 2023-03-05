<template>
    <div class="goal-input">
        <textarea v-model="goal" rows="5"></textarea>
        <button @click="saveGoal">Save</button>
    </div>
</template>
  
<script>
export default {
    data() {
        return {
            goal: ''
        }
    },
    methods: {
        saveGoal() {
            if (this.goal.trim() !== '') {
                this.$emit('save-goal', this.goal);
                this.goal = '';
            }
        }
    }
}
</script>
  
<style scoped>
.goal-input {
    margin-top: 20px;
    display: flex;
    flex-direction: column;
}

.goal-input textarea {
    margin-bottom: 10px;
}
</style>
  