<script setup>
const trainings = [
  { id: 1, name: "BJJ Gi", tag: "Montag" },
  { id: 2, name: "No-Gi", tag: "Mittwoch" },
  { id: 3, name: "Open Mat", tag: "Samstag" }
]
</script>

<template>
  <div>
    <h2>Meine Trainingseinheiten</h2>

    <ul>
      <li v-for="training in trainings" :key="training.id">
        {{ training.name }} - {{ training.tag }}
      </li>
    </ul>
  </div>
</template>


//Kalender Verbindung

<script setup>
import CalendarView from './views/CalendarView.vue'
</script>

<template>
  <main>
    <h1>MatConnect</h1>

    <CalendarView />
  </main>
</template>