<template>
  <div class="habit-card">
    <Checkbox @toggle="emit('toggle')" :is-checked="isChecked"></Checkbox>
    <h2 class="habit-card__title">{{ habit }}</h2>
  </div>
</template>

<script setup>
import Checkbox from '../atoms/Checkbox.vue';

const emit = defineEmits(['toggle'])

const props = defineProps({
  habit: String,
  isChecked: Boolean
})
</script>

<style scoped>
.habit-card {
  background-color: var(--color-card);
  border: 3px solid var(--color-border);
  border-radius: var(--radius-lg);
  padding: 20px;
  display: flex;
  align-items: center;
  gap: 20px;
  width: 350px;
  box-shadow: 4px 4px 0px var(--color-border);
}

.habit-card__title {
  font-family: 'Inter', sans-serif;
  font-weight: 800;
  font-size: 1.2rem;
  text-transform: uppercase;
  letter-spacing: -0.03em;
  color: var(--color-text);
  margin: 0;
}
</style>