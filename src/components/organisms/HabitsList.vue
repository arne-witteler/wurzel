<template>
  <ul class="habitlist">
    <li class="habitlist__item" :key="habit.id" v-for="habit in habitlist">
      <HabitCard @toggle="toggleHabit(habit.id)" :is-checked="habit.ischecked" :habit="habit.habit"></HabitCard>
    </li>
  </ul>
</template>

<script setup>
import { ref } from 'vue';
import HabitCard from '../molecules/HabitCard.vue';
import habits from '../../data/habits.json';

const habitlist = ref(habits);

function toggleHabit(id) {
  const findHabit = habitlist.value.find((item) => item.id === id);

  if (findHabit) {
    findHabit.ischecked = !findHabit.ischecked;
  }
}
</script>

<style scoped>
.habitlist {
  display: flex;
  flex-direction: column;
  gap: 24px;
  padding: 20px;
}
</style>