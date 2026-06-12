<template>
  <header class="app-header">
    <div class="app-header__branding">
      <h1 class="app-header__title">Wurzel</h1>
      <p class="app-header__sub-title">The habit tracker</p>
    </div>

    <button class="app-header__burger" type="button">
      <img src="../../assets/icons/BurgerMenuIcon.svg" alt="Menü" class="app-header__burger-icon">
    </button>
  </header>
  <div class="habits-container">
    <div class="progress-monitor">
      <span class="progress-monitor__date">Friday, Jun 12</span>

      <div class="progress-monitor__status">
        <span class="progress-monitor__count-done">{{ numberOfCheckedHabits }}</span>
        <span class="progress-monitor__count-total">{{ numberOfHabits }}</span>
        <span class="progress-monitor__label">DONE</span>
      </div>

      <div class="progress-bar">
        <div class="progress-bar__fill" :style="{ width: percentageOfCheckedHabits + '%' }"></div>
      </div>

      <div class="progress-labels">
        <span>0%</span>
        <span>{{ Math.round(percentageOfCheckedHabits) + ' %' }}</span>
        <span>100%</span>
      </div>
    </div>
    <ul class="filter-tabs">
      <li v-for="daytime in daytimes" :key="daytime" class="filter-tabs__item">
        <button @click="currentFilter = daytime" class="filter-tabs__button"
          :class="{ 'filter-tabs__button--active': currentFilter === daytime }" type="button">
          {{ daytime }}
        </button>
      </li>
    </ul>

    <ul class="habit-list">
      <li v-for="habit in filteredHabits" :key="habit.id" class="habit-list__item">
        <HabitCard @toggle="toggleHabit(habit.id)" :is-checked="habit.ischecked" :habit="habit.habit"
          :daytime="habit.daytime"></HabitCard>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import HabitCard from '../molecules/HabitCard.vue';
import habits from '../../data/habits.json';

const habitlist = ref(habits);
const currentFilter = ref('all');

const daytimes = new Set(habitlist.value.map((item) => item.daytime));

const filteredHabits = computed(() => {
  if (currentFilter.value === "all") {
    return habitlist.value;
  } else {
    return habitlist.value.filter((item => item.daytime === currentFilter.value));
  }

})

function toggleHabit(id) {
  const targetHabit = habitlist.value.find((item) => item.id === id);

  if (targetHabit) {
    targetHabit.ischecked = !targetHabit.ischecked;
  }
}

const numberOfHabits = computed(() => habitlist.value.length);
const numberOfCheckedHabits = computed(() => habitlist.value.filter((item => item.ischecked === true)).length);
const percentageOfCheckedHabits = computed(() => numberOfCheckedHabits.value / numberOfHabits.value * 100);

</script>

<style scoped>
.app-header {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
  border-bottom: 2px solid var(--color-black);
  font-family: 'Inter', sans-serif;
}

.app-header__branding {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.app-header__title {
  font-size: 1.8rem;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  line-height: 1;
  margin: 0 0 4px 0;
}

.app-header__sub-title {
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--color-grey);
  margin: 0;
  text-transform: uppercase;
  letter-spacing: 0.03em;
}

.app-header__burger {
  background: none;
  border: none;
  padding: 0;
  width: 28px;
  height: 24px;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 6px;
}

.app-header__burger-line {
  display: block;
  width: 100%;
  height: 3px;
  background-color: var(--color-black);
  transition: transform 0.2s ease, opacity 0.2s ease;
}

.app-header__burger :last-child {
  width: 80%;
  align-self: flex-end;
}

.habits-container {
  width: 100%;
  max-width: 400px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.progress-monitor {
  width: 100%;
  padding: 20px 0;
}

.progress-monitor__date {
  font-size: 0.8rem;
  font-weight: 700;
  text-transform: uppercase;
  color: var(--color-grey);
  letter-spacing: 0.05em;
}

.progress-monitor__status {
  margin: 10px 0;
  display: flex;
  align-items: baseline;
  gap: 6px;
}

.progress-monitor__count-done {
  font-size: 3rem;
  font-weight: 900;
  line-height: 1;
}

.progress-monitor__count-total {
  font-size: 1.5rem;
  font-weight: 800;
  color: var(--color-black);
}

.progress-monitor__label {
  font-size: 1.1rem;
  font-weight: 800;
  letter-spacing: 0.05em;
  margin-left: 4px;
}

.progress-bar {
  width: 100%;
  height: 6px;
  background-color: #e0e0e0;
  position: relative;
  margin-bottom: 8px;
}

.progress-bar__fill {
  height: 100%;
  background-color: var(--color-black);
  transition: width 0.2s ease;
}

.progress-labels {
  display: flex;
  justify-content: space-between;
  font-size: 0.75rem;
  font-weight: 700;
  color: #8a8a8a;
  text-transform: uppercase;
}

.progress-labels__text {
  color: var(--color-black);
  font-weight: 800;
}

.filter-tabs {
  display: flex;
  list-style: none;
  padding: 0;
  margin: 0 0 20px 0;
  border-bottom: 1px solid var(--color-black);
  border-top: 1px solid var(--color-black);
}

.filter-tabs__item {
  margin-right: 5px;
}

.filter-tabs__button {
  background: none;
  border: none;
  font-weight: 800;
  font-size: 0.85rem;
  text-transform: uppercase;
  color: var(--color-grey);
  padding: 12px 20px;
  cursor: pointer;
  position: relative;
  transition: color 0.1s ease;
}

.filter-tabs__button:hover {
  color: var(--color-black);
}

.filter-tabs__button--active {
  color: var(--color-black);
  border-bottom: 1px solid var(--color-black);
}

.habit-list {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 0;
  list-style: none;
}
</style>