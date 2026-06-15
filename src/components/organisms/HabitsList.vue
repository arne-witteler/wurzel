<template>
  <div class="app-layout">
    <header class="app-header">
      <div class="app-header__branding">
        <h1 class="app-header__title">Wurzel</h1>
        <p class="app-header__sub-title">The habit tracker</p>
      </div>
      <button @click="isMenuOpen = !isMenuOpen" class="app-header__burger" type="button">
        <img src="@/assets/icons/BurgerMenuIcon.svg" alt="Menü" class="app-header__burger-icon">
      </button>
      <div class="app-menu" v-if="isMenuOpen">
        <ul>
          <li>
            <p>Statistics</p>
          </li>
          <li>
            <p>Settings</p>
          </li>
          <li>
            <p>Export Data</p>
          </li>
          <li>
            <p>About</p>
          </li>
        </ul>
      </div>
    </header>
    <div class="habits-container">
      <div class="progress-monitor">
        <span class="progress-monitor__date">{{ date }}</span>
        <div class="progress-monitor__status">
          <span class="progress-monitor__count-done">{{ numberOfCheckedHabits }}</span>
          <span class="progress-monitor__count-total">{{ '/ ' + numberOfHabits }}</span>
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
      <div class="habits-content-block">
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
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';
import HabitCard from '../molecules/HabitCard.vue';
import habits from '../../data/habits.json';

const habitlist = ref(habits);
const currentFilter = ref('all');
const isMenuOpen = ref(false);

const daytimes = ["all", "morning", "noon", "evening", "anytime"];
const options = { weekday: 'long', month: 'short', day: 'numeric' };
const date = new Date().toLocaleDateString('en-US', options).toUpperCase();

const filteredHabits = computed(() => {
  if (currentFilter.value === "all") {
    return habitlist.value;
  } else {
    return habitlist.value.filter((item => item.daytime === currentFilter.value));
  }
});

function toggleHabit(id) {
  const targetHabit = habitlist.value.find((item) => item.id === id);
  if (targetHabit) {
    targetHabit.ischecked = !targetHabit.ischecked;
  }
}

const numberOfHabits = computed(() => habitlist.value.length);
const numberOfCheckedHabits = computed(() => habitlist.value.filter((item => item.ischecked === true)).length);
const percentageOfCheckedHabits = computed(() => (numberOfCheckedHabits.value / numberOfHabits.value) * 100 || 0);
</script>

<style scoped>
.app-layout {
  width: 100%;
  max-width: 400px;
  margin: 0 auto;
}

.app-header {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 16px;
  border-bottom: 2px solid var(--color-black);
  position: relative;
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
}

.app-menu {
  position: absolute;
  top: 100%;
  right: 16px;
  width: 200px;
  background-color: var(--color-white);
  border: 2px solid var(--color-black);
  z-index: 100;
  margin-top: 8px;
}

.app-menu ul {
  padding: 0;
  margin: 0;
}

.app-menu li {
  padding: 12px 16px;
  cursor: pointer;
}

.app-menu li:hover {
  background-color: var(--color-black);
  color: var(--color-white);
}

.app-menu p {
  margin: 0;
  font-weight: 800;
  text-transform: uppercase;
  font-size: 0.85rem;
}

.habits-container {
  width: 100%;
  padding: 0 16px;
  display: flex;
  flex-direction: column;
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
  background-color: var(--color-light-grey);
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
  color: var(--color-grey);
  text-transform: uppercase;
}

.filter-tabs {
  display: flex;
  overflow-x: auto;
  scroll-behavior: smooth;
  -webkit-overflow-scrolling: touch;
  list-style: none;
  padding: 0;
  margin: 0 0 20px 0;
  border-bottom: 1px solid var(--color-black);
  border-top: 1px solid var(--color-black);
}

.filter-tabs::-webkit-scrollbar {
  display: none;
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
  padding: 12px 10px;
  cursor: pointer;
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

@media (min-width: 768px) {

  .app-layout {
    max-width: 850px;
  }

  .app-header {
    padding: 20px 0;
  }

  .habits-container {
    display: grid;
    grid-template-columns: 280px 1fr;
    gap: 48px;
    padding: 24px 0 0 0;
  }

  .progress-monitor {
    position: sticky;
    top: 20px;
    height: fit-content;
    padding: 0;
  }
}

@media (min-width: 1024px) {
  .app-layout {
    max-width: 1000px;
  }

  .habits-container {
    grid-template-columns: 320px 1fr;
    gap: 80px;
  }
}
</style>