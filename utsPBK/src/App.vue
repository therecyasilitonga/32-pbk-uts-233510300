<template>
  <div class="gacor-app">
    <header>
      <h1>🚀 Kegiatan Teknologi Masa Depan</h1>
      <p>Temukan perkembangan teknologi yang mencengangkan!</p>
    </header>

    <!-- Form untuk menambah kegiatan belajar -->
    <div class="add-task">
      <input v-model="newTask" placeholder="Contoh: Belajar Blockchain" @keyup.enter="addTask" />
      <button @click="addTask">Tambah Kegiatan</button>
    </div>

    <!-- Filter kegiatan yang belum selesai -->
    <div class="filter">
      <label>
        <input type="checkbox" v-model="showIncompleteOnly" />
        Tampilkan hanya yang belum selesai
      </label>
    </div>

    <!-- Tombol untuk reset semua kegiatan -->
    <button class="reset-btn" @click="resetTasks">Reset Semua Kegiatan</button>

    <!-- Daftar Kegiatan -->
    <ul class="task-list">
      <transition-group name="list" tag="ul">
        <li v-for="(task, index) in filteredTasks" :key="task.id" class="task-item">
          <input type="checkbox" v-model="task.completed" />
          <span :class="{ completed: task.completed }">{{ task.text }}</span>
          <button @click="removeTask(index)">❌</button>
        </li>
      </transition-group>
    </ul>

    <!-- Notifikasi ketika kegiatan ditambahkan -->
    <div v-if="notification" class="notification">
      <p>{{ notification }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTask = ref("")
const tasks = ref([])
const showIncompleteOnly = ref(false)
const notification = ref("")  // Notifikasi untuk feedback

// Fungsi untuk menambahkan kegiatan belajar
function addTask() {
  if (newTask.value.trim()) {
    const task = { id: Date.now(), text: newTask.value, completed: false }
    tasks.value.push(task)
    notification.value = "Kegiatan berhasil ditambahkan!"
    setTimeout(() => notification.value = "", 3000)  // Notifikasi hilang setelah 3 detik
    newTask.value = ""
  }
}

// Fungsi untuk menghapus kegiatan
function removeTask(index) {
  tasks.value.splice(index, 1)
  notification.value = "Kegiatan berhasil dihapus!"
  setTimeout(() => notification.value = "", 3000)  // Notifikasi hilang setelah 3 detik
}

// Fungsi untuk reset semua kegiatan
function resetTasks() {
  tasks.value = []
  notification.value = "Semua kegiatan telah direset!"
  setTimeout(() => notification.value = "", 3000)  // Notifikasi hilang setelah 3 detik
}

// Filter kegiatan yang belum selesai
const filteredTasks = computed(() =>
  showIncompleteOnly.value
    ? tasks.value.filter(task => !task.completed)
    : tasks.value
)
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap');

.gacor-app {
  background: linear-gradient(45deg, #2f3b52, #1a1e25);
  color: white;
  font-family: 'Orbitron', sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh;
  padding: 3rem;
  text-align: center;
  box-shadow: 0px 4px 15px rgba(0, 0, 0, 0.2);
  animation: backgroundTransition 40s ease-in-out infinite;
  position: relative;
  overflow: hidden;
  transition: all 0.5s ease-in-out;
}

/* Menambahkan efek latar belakang bintang bergerak perlahan */
.gacor-app::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: url('https://www.transparenttextures.com/patterns/stardust.png');
  animation: moveStars 240s linear infinite;
  opacity: 0.3;
  pointer-events: none;
}

header h1 {
  font-size: 3.5rem;
  font-weight: 700;
  margin-bottom: 1rem;
  color: #ffcc00;
  text-shadow: 0 4px 6px rgba(0, 0, 0, 0.3);
  animation: glow 1.5s ease-in-out infinite alternate;
}

header p {
  font-size: 1.2rem;
  color: #fff;
  margin-bottom: 2rem;
  font-weight: 600;
}

.add-task {
  display: flex;
  gap: 1.5rem;
  margin-bottom: 2rem;
  animation: slideIn 0.6s ease-out;
}

.add-task input {
  padding: 1rem 1.5rem;
  font-size: 1.2rem;
  border-radius: 12px;
  border: 2px solid #ff80ab;
  width: 300px;
  background-color: #ffffff;
  color: #333;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.3s ease;
}

.add-task input::placeholder {
  color: #b3b3b3;
}

.add-task input:focus {
  box-shadow: 0 0 10px rgba(255, 128, 171, 0.7);
}

.add-task button {
  padding: 1rem 2rem;
  background-color: #ff80ab;
  color: white;
  font-weight: 700;
  border: none;
  border-radius: 12px;
  cursor: pointer;
  transition: transform 0.3s ease, background-color 0.3s ease;
}

.add-task button:hover {
  background-color: #ff3399;
  transform: scale(1.1);
}

.filter {
  margin-bottom: 2rem;
  font-size: 1.2rem;
  color: #fff;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
  width: 100%;
  max-width: 650px;
  animation: fadeIn 1s ease-in-out;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1rem;
  background: #ffffff;
  margin-bottom: 1rem;
  border-radius: 15px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  transition: transform 0.3s ease, background-color 0.3s ease;
}

.task-item:hover {
  transform: scale(1.05);
  background-color: #ffebef;
}

.task-item input[type="checkbox"] {
  transform: scale(1.5);
  accent-color: #ff80ab;
}

.task-item span {
  flex-grow: 1;
  font-size: 1.3rem;
  font-weight: 500;
  color: #333;
  transition: color 0.3s ease;
}

.task-item span.completed {
  text-decoration: line-through;
  color: #ff4081;
}

.task-item button {
  background: none;
  border: none;
  color: #ff4081;
  font-size: 1.5rem;
  cursor: pointer;
  transition: color 0.3s ease;
}

.task-item button:hover {
  color: #ff00ff;
}

.reset-btn {
  margin-top: 2rem;
  padding: 1rem 2rem;
  background-color: #ff4081;
  color: white;
  font-weight: 700;
  border: none;
  border-radius: 12px;
  cursor: pointer;
  transition: transform 0.3s ease, background-color 0.3s ease;
}

.reset-btn:hover {
  background-color: #ff1e70;
  transform: scale(1.1);
}

.notification {
  position: absolute;
  top: 20px;
  right: 20px;
  background: rgba(0, 0, 0, 0.7);
  color: #fff;
  padding: 10px 20px;
  border-radius: 10px;
  font-size: 1rem;
  opacity: 0.9;
  transition: opacity 0.3s ease-out;
}

.notification p {
  margin: 0;
}

/* Animasi untuk latar belakang bergerak bintang */
@keyframes moveStars {
  0% {
    background-position: 0 0;
  }
  100% {
    background-position: 2000px 2000px;
  }
}

@keyframes glow {
  0% {
    text-shadow: 0 4px 6px rgba(0, 0, 0, 0.3), 0 0 25px #ff0099;
  }
  100% {
    text-shadow: 0 4px 6px rgba(0, 0, 0, 0.3), 0 0 25px #ff80ab;
  }
}

@keyframes slideIn {
  from {
    transform: translateY(20px);
    opacity: 0;
  }
  to {
    transform: translateY(0);
    opacity: 1;
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

</style>
