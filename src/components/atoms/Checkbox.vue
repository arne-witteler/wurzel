<template>
  <button @click="emit('toggle')" class="checkbox" :class="{ 'checkbox--checked': isChecked }"></button>
</template>

<script setup>

const emit = defineEmits(['toggle'])

const props = defineProps({
  isChecked: Boolean,
})


</script>

<style scoped>
.checkbox {
  background-color: var(--color-card);
  border: 3px solid var(--color-border);
  border-radius: var(--radius-lg);
  width: 34px;
  height: 34px;
  cursor: pointer;
  transition: transform 0.1s ease, box-shadow 0.1s ease;
  box-shadow: 2px 2px 0px var(--color-border);
}

.checkbox:active {
  transform: translate(1px, 1px);
  box-shadow: 1px 1px 0px var(--color-border);
}

.checkbox--checked {
  background-color: var(--color-border);
}
</style>