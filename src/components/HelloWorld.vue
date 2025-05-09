<script setup>
import { ref } from 'vue'

const activities = ref(['makan', 'mandi', 'olahraga', 'tidur', 'kuliah'])
const newActivity = ref('')
const doneActivities = ref(new Set())

function addActivity() {
  if (newActivity.value.trim() !== '') {
    activities.value.push(newActivity.value.trim())
    newActivity.value = ''
  }
}

function removeActivity(index) {
  activities.value.splice(index, 1)
  doneActivities.value.delete(index)
  // Adjust doneActivities set for shifted indices
  const newDone = new Set()
  doneActivities.value.forEach(i => {
    if (i > index) {
      newDone.add(i - 1)
    } else if (i < index) {
      newDone.add(i)
    }
  })
  doneActivities.value = newDone
}

function toggleDone(index) {
  if (doneActivities.value.has(index)) {
    doneActivities.value.delete(index)
  } else {
    doneActivities.value.add(index)
  }
}
</script>

<template>
  <div>
    <h2>Todo List</h2>
    <ul>
      <li v-for="(activity, index) in activities" :key="index">
        <input type="checkbox" :checked="doneActivities.has(index)" @change="toggleDone(index)" />
        <span>{{ activity }}</span>
        <button @click="removeActivity(index)">Hapus</button>
      </li>
    </ul>
    <div>
      <label for="activityInput">Tambah Kegiatan:</label>
      <input id="activityInput" type="text" v-model="newActivity" />
      <button type="submit" @click="addActivity">Submit</button>
    </div>
  </div>
</template>
