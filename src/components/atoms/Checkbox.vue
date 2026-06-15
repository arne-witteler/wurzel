<template>
  <button @click="emit('toggle')" class="checkbox" :class="{ 'checkbox--checked': isChecked }"><img v-if="isChecked"
      src="../../assets/icons/CheckIcon.svg" class="checkbox__icon"></button>
</template>

<script setup>

const emit = defineEmits(['toggle'])

const props = defineProps({
  isChecked: Boolean,
})
</script>

<style scoped>
.checkbox {
  display: flex;
  background-color: var(--color-card);
  border: 1px solid var(--color-black);
  border-radius: var(--radius-lg);
  width: 34px;
  height: 34px;
  cursor: pointer;
  transition: transform 0.1s ease, box-shadow 0.1s ease;
  flex-shrink: 0;
}

.checkbox:active {
  transform: translate(1px, 1px);
}

.checkbox--checked {
  background-color: var(--color-black);
}

.checkbox__icon {
  padding: 5px;
}
</style>