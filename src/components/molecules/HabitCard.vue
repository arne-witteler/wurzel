<template>
  <div class="habit-card">
    <Checkbox @toggle="emit('toggle')" :is-checked="isChecked"></Checkbox>
    <h2 class="habit-card__title">{{ habit }}</h2>
    <div class="habit-card__meta">
      <p class="habit-card__time">{{ daytime }}</p>
    </div>
  </div>
</template>

<script setup>
import Checkbox from '../atoms/Checkbox.vue';

const emit = defineEmits(['toggle'])

const props = defineProps({
  habit: String,
  daytime: String,
  isChecked: Boolean
})
</script>

<style scoped>
.habit-card {
  background-color: var(--color-card);
  border: 1px solid var(--color-black);
  border-radius: var(--radius-lg);
  padding: 16px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
  width: 100%;
}

.habit-card__title {
  font-weight: 800;
  font-size: 1.2rem;
  text-transform: uppercase;
  letter-spacing: -0.03em;
  color: var(--color-text);
  margin: 0;
}

.habit-card__meta {
  border: 1px solid var(--color-black);
  padding: 5px;
}

.habit-card__time {
  text-transform: uppercase;
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 0.1em;
}
</style>