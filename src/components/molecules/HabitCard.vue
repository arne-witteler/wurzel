<template>
  <div class="habit-card">
    <div class="habit-card__checkbox-wrapper">
      <Checkbox @toggle="emit('toggle')" :is-checked="isChecked"></Checkbox>
    </div>

    <div class="habit-card__content">
      <h2 class="habit-card__title" :class="{ 'habit-card__title--checked': isChecked }">
        {{ habit }}
      </h2>
      <p v-if="streakCount >= 1" class="habit-card__streak">+{{ streakCount }} DAYS STREAK</p>
      <p v-else class="habit-card__streak">NO STREAK</p>

      <div class="habit-card__history-grid">
        <div v-for="day in currentWeekDays" :key="day" class="history-cube"
          :class="{ 'history-cube--checked': history.includes(day) }"></div>
      </div>
    </div>

    <div class="habit-card__meta">
      <p class="habit-card__time">{{ daytime }}</p>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue';
import Checkbox from '../atoms/Checkbox.vue';

const emit = defineEmits(['toggle'])

const props = defineProps({
  habit: String,
  daytime: String,
  isChecked: Boolean,
  history: {
    type: Array,
    default: () => []
  }
})

const currentWeekDays = computed(() => {
  const days = [];
  const today = new Date();

  const currentWeekday = today.getDay();
  const daysSinceMonday = currentWeekday === 0 ? 6 : currentWeekday - 1;

  const monday = new Date(today);
  monday.setDate(today.getDate() - daysSinceMonday);

  for (let i = 0; i < 7; i++) {
    const nextDay = new Date(monday);
    nextDay.setDate(monday.getDate() + i);

    const day = nextDay.toISOString().split('T')[0];
    days.push(day);
  }

  return days;
})

const streakCount = computed(() => {
  let streak = 0;
  const checkDate = new Date();

  const todayStr = checkDate.toISOString().split('T')[0];
  if (!props.history.includes(todayStr)) {
    checkDate.setDate(checkDate.getDate() - 1);
  }

  while (true) {
    const dateStr = checkDate.toISOString().split('T')[0];
    if (props.history.includes(dateStr)) {
      streak++;
      checkDate.setDate(checkDate.getDate() - 1);
    } else {
      break;
    }
  }

  return streak;
})
</script>

<style scoped>
.habit-card {
  background-color: var(--color-card);
  border: 1px solid var(--color-black);
  border-radius: var(--radius-lg);
  padding: 16px;
  display: grid;
  grid-template-columns: auto 1fr auto;
  align-items: start;
  gap: 16px;
  width: 100%;
}

.habit-card__checkbox-wrapper {
  display: flex;
  align-items: start;
}

.habit-card__content {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: flex-start;
  text-align: left;
}

.habit-card__title {
  font-weight: 800;
  font-size: 0.9rem;
  text-transform: uppercase;
  letter-spacing: -0.03em;
  color: var(--color-text);
  margin: 0;
  line-height: 1.2;
}

.habit-card__title--checked {
  text-decoration: line-through;
  color: var(--color-grey);
}

.habit-card__streak {
  font-size: 0.6rem;
  font-weight: 700;
  color: var(--color-grey);
  text-transform: uppercase;
  letter-spacing: 0.05em;
  margin: 6px 0 10px 0;
}

.habit-card__history-grid {
  display: flex;
  gap: 4px;
}

.history-cube {
  width: 12px;
  height: 12px;
  background-color: var(--color-white);
  border: 1px solid var(--color-black);
  border-radius: 1px;
}

.history-cube--checked {
  background-color: var(--color-black);
}

.habit-card__meta {
  border: 1px solid var(--color-black);
  padding: 5px 8px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.habit-card__time {
  text-transform: uppercase;
  font-size: 0.5rem;
  font-weight: 800;
  letter-spacing: 0.1em;
  margin: 0;
}
</style>