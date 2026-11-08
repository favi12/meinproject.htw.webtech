<script setup>
import { ref, computed } from 'vue'

// Aktuell angezeigter Monat
const aktuellesDatum = ref(new Date())

// Beispiel-Trainingseinheiten
const trainings = ref([
  { id: 1, name: 'BJJ Gi', datum: '2026-11-09' },
  { id: 2, name: 'No-Gi', datum: '2026-11-11' },
  { id: 3, name: 'Open Mat', datum: '2026-11-14' }
])

// Monat und Jahr für die Überschrift
const monatTitel = computed(() =>
  aktuellesDatum.value.toLocaleDateString('de-DE', {
    month: 'long',
    year: 'numeric'
  })
)

// Alle Kalendertage berechnen
const kalenderTage = computed(() => {
  const jahr = aktuellesDatum.value.getFullYear()
  const monat = aktuellesDatum.value.getMonth()

  const anzahlTage = new Date(jahr, monat + 1, 0).getDate()
  const ersterWochentag = (new Date(jahr, monat, 1).getDay() + 6) % 7

  const tage = []

  // Leere Felder vor dem ersten Monatstag
  for (let i = 0; i < ersterWochentag; i++) {
    tage.push(null)
  }

  // Tage des Monats
  for (let tag = 1; tag <= anzahlTage; tag++) {
    const datum = `${jahr}-${String(monat + 1).padStart(2, '0')}-${String(tag).padStart(2, '0')}`

    tage.push({
      tag,
      datum,
      trainings: trainings.value.filter(t => t.datum === datum)
    })
  }

  return tage
})

// Zwischen Monaten wechseln
function monatWechseln(richtung) {
  const datum = aktuellesDatum.value

  aktuellesDatum.value = new Date(
    datum.getFullYear(),
    datum.getMonth() + richtung,
    1
  )
}
</script>

<template>
  <section class="kalender">
    <h2>Trainingskalender</h2>

    <div class="navigation">
      <button @click="monatWechseln(-1)">←</button>
      <h3>{{ monatTitel }}</h3>
      <button @click="monatWechseln(1)">→</button>
    </div>

    <div class="kalender-grid">
      <div v-for="tag in ['Mo', 'Di', 'Mi', 'Do', 'Fr', 'Sa', 'So']"
           :key="tag" class="wochentag">
        {{ tag }}
      </div>

      <div v-for="(tag, index) in kalenderTage"
           :key="index"
           class="kalender-tag">

        <template v-if="tag">
          <strong>{{ tag.tag }}</strong>

          <div v-for="training in tag.trainings"
               :key="training.id"
               class="training">
            {{ training.name }}
          </div>
        </template>

      </div>
    </div>
  </section>
</template>

<style scoped>
.kalender {
  max-width: 850px;
  margin: 30px auto;
  font-family: Arial, sans-serif;
}

.navigation {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.navigation button {
  padding: 10px 16px;
  cursor: pointer;
}

.kalender-grid {
  display: grid;
  grid-template-columns: repeat(7, minmax(0, 1fr));
  gap: 5px;
}

.wochentag {
  text-align: center;
  font-weight: bold;
  padding: 10px 0;
}

.kalender-tag {
  min-height: 75px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
  min-width: 0;
}

.training {
  margin-top: 5px;
  padding: 4px;
  border-radius: 4px;
  background-color: #dbeafe;
  color: #1e40af;
  font-size: 12px;
  overflow-wrap: anywhere;
}
</style>


