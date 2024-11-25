<script setup lang="ts">

import { ref } from 'vue';
import CalendarContainer from './components/CalendarContainer.vue';

const calendarOpen = ref(true)
</script>

<template>
  <button @click="calendarOpen = !calendarOpen" id="calendarToggleButton">{{ calendarOpen ? 'Zapri' : 'Odpri' }} koledar</button>
  <CalendarContainer v-if="calendarOpen"></CalendarContainer>
</template>

<style scoped>

#calendarToggleButton {
  all: unset;
  font-weight: bold;
  padding: 0.75rem 1rem;
  font-size: 0.875rem;
  border-radius: 2rem;
  background: #f8f8ff;
  border: 1px solid #58585f;
  transition: background-color 0.25s ease, color 0.25s ease;
  cursor: pointer;
  position: absolute;
  top: 1rem;
  left: 2rem;
}

@media screen and (max-width: 900px) {
  #calendarToggleButton {
    left: 0.5rem;
    top: 0.5rem;
  }
}

#calendarToggleButton:hover {
  background: #08080f;
  color: #f8f8ff;
}

</style>
