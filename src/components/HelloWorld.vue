<script setup>
import { ref, computed } from 'vue'

const activities = ref(['makan', 'mandi', 'olahraga', 'tidur', 'kuliah'])
const newActivity = ref('')
const doneActivities = ref(new Set())
const showIncompleteOnly = ref(false)

function addActivity() {
  if (newActivity.value.trim() !== '') {
    activities.value.push(newActivity.value.trim())
    newActivity.value = ''
  }
}

function removeActivity(index) {
  activities.value.splice(index, 1)
  doneActivities.value.delete(index)
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

const filteredActivities = computed(() => {
  if (showIncompleteOnly.value) {
    return activities.value.filter((_, index) => !doneActivities.value.has(index))
  }
  return activities.value
})
</script>

<template>
  <div class="todo-container">
    <h2>Todo List</h2>
    <button class="toggle-btn" @click="showIncompleteOnly = !showIncompleteOnly">
      {{ showIncompleteOnly ? 'Tampilkan Semua' : 'Tampilkan Belum Selesai' }}
    </button>
    <ul class="todo-list">
      <li v-for="(activity, index) in filteredActivities" :key="index" class="todo-item">
        <input type="checkbox" :checked="doneActivities.has(index)" @change="toggleDone(index)" />
        <span :class="{ done: doneActivities.has(index) }">{{ activity }}</span>
        <button class="remove-btn" @click="removeActivity(index)">Hapus</button>
      </li>
    </ul>
    <div class="add-activity">
      <label for="activityInput">Tambah Kegiatan:</label>
      <input id="activityInput" type="text" v-model="newActivity" placeholder="Masukkan kegiatan baru" />
      <button type="submit" @click="addActivity" :disabled="newActivity.trim() === ''">Submit</button>
    </div>
  </div>
</template>

<style scoped>
.todo-container {
  max-width: 400px;
  margin: 2rem auto;
  padding: 1.5rem;
  background: #f9f9f9;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: #333;
}

h2 {
  text-align: center;
  margin-bottom: 1rem;
  color: #2c3e50;
}

.toggle-btn {
  display: block;
  margin: 0 auto 1rem auto;
  padding: 0.5rem 1rem;
  background-color: #3498db;
  border: none;
  border-radius: 20px;
  color: white;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.toggle-btn:hover {
  background-color: #2980b9;
}

.todo-list {
  list-style: none;
  padding: 0;
  margin: 0 0 1rem 0;
}

.todo-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.5rem 0;
  border-bottom: 1px solid #ddd;
}

.todo-item:last-child {
  border-bottom: none;
}

.todo-item input[type="checkbox"] {
  margin-right: 0.75rem;
  width: 18px;
  height: 18px;
  cursor: pointer;
}

.todo-item span {
  flex-grow: 1;
  font-size: 1rem;
}

.todo-item span.done {
  text-decoration: line-through;
  color: #999;
}

.remove-btn {
  background-color: #e74c3c;
  border: none;
  border-radius: 12px;
  color: white;
  padding: 0.3rem 0.7rem;
  cursor: pointer;
  font-size: 0.85rem;
  transition: background-color 0.3s ease;
}

.remove-btn:hover {
  background-color: #c0392b;
}

.add-activity {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.add-activity label {
  font-weight: 600;
  color: #34495e;
}

.add-activity input[type="text"] {
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 1rem;
  outline: none;
  transition: border-color 0.3s ease;
}

.add-activity input[type="text"]:focus {
  border-color: #3498db;
}

.add-activity button {
  align-self: flex-start;
  padding: 0.5rem 1rem;
  background-color: #27ae60;
  border: none;
  border-radius: 20px;
  color: white;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.add-activity button:hover {
  background-color: #1e8449;
}
</style>
