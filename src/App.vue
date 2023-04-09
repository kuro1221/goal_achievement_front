<template>
  <v-app>
    <v-main>
      <v-container>
        <div style="overflow-x: auto;">
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th>User</th>
                  <th>Goal</th>
                  <th v-for="n in 31" :key="n">{{ n }}</th>
                </tr>
              </thead>
              <tbody>
                <!-- <tr v-for="(user, index) in users" :key="index">
                  <td>{{ user.name }}</td>
                  <td v-for="day in 31" :key="day">
                    <v-select :items="goalStatus" v-model="user.goal[day - 1]" outlined small hide-details></v-select>
                  </td>
                </tr> -->
                <tr v-for="(goal, index) in state.achievements.goals" :key="index">
                  <td>{{ state.achievements.name }}</td>
                  <td>{{ goal.title }}</td>
                  <td v-for="day in getDaysInMonth(new Date().getFullYear(), new Date().getMonth())" :key="day">
                    <v-select :items="goalStatus" :value="judgeGoalStatus(goal, day)" outlined small hide-details>
                    </v-select>
                  </td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </div>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { ref, reactive } from 'vue'
import axios from 'axios'

export default {
  setup() {
    const users = ref([
      { name: 'Alice', goal: Array(31).fill(null) },
      { name: 'Bob', goal: Array(31).fill(null) },
      { name: 'Carol', goal: Array(31).fill(null) },
    ])

    const state = reactive({
      achievements: []
    });

    const goalStatus = ['-', '○', '×', '休']

    const fetchUserData = async () => {
      await new Promise(resolve => setTimeout(resolve, 1000))
      return [
        { name: 'Alice', goal: Array(31).fill(null) },
        { name: 'Bob', goal: Array(31).fill(null) },
        { name: 'Carol', goal: Array(31).fill(null) },
      ]
    }

    const judgeGoalStatus = (goal, day) => {
      const date = new Date(2023, 3, day).toDateString(); // 指定した月の日にちに関する日付を取得

      const achievement = goal.achievements.find(a => new Date(a.date).toDateString() == date); // 指定した日にちの目標達成状況を取得

      if (!achievement) { // 指定した日にちの目標達成状況がない場合
        return goalStatus[0];
      } else if (achievement.status == '1') { // 達成
        return goalStatus[1];
      } else if (achievement.status == '2') { // 未達成
        return goalStatus[2];
      } else if (achievement.status == '3') { // 休み
        return goalStatus[3];
      }

      return goalStatus[1];
    }

    const fetchAchievementData = async () => {
      axios.get('http://localhost/api/achievement/1')
        .then(response => {
          console.log(response.data.data[0])
          state.achievements = response.data.data[0];
        })
        .catch(error => {
          console.log(error);
        });
    }

    // 指定した月の日数を取得する関数
    const getDaysInMonth = (year, month) => {
      return new Date(year, month + 1, 0).getDate();
    }

    fetchAchievementData()
    fetchUserData().then(data => (users.value = data))

    return { users, goalStatus, state, judgeGoalStatus, getDaysInMonth }
  },
}
</script>
