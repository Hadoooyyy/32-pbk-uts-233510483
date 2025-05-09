<script setup>
import { ref } from 'vue'

const activities = ref(['makan', 'mandi', 'olahraga', 'tidur', 'kuliah'])
const newActivity = ref('')

function addActivity() {
  if (newActivity.value.trim() !== '') {
    activities.value.push(newActivity.value.trim())
    newActivity.value = ''
  }
}

function removeActivity(index) {
  activities.value.splice(index, 1)
}
</script>

<template>
  <div>
    <h2>Todo List</h2>
    <ul>
      <li v-for="(activity, index) in activities" :key="index">
        {{ activity }}
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
