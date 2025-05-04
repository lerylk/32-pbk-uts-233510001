<template>
  <div class="todo-app">
    <h1>Daftar Kegiatan</h1>

    <form @submit.prevent="addTask" class="input-form" aria-label="Form tambah kegiatan baru">
      <input
        v-model="newTask"
        type="text"
        placeholder="Tambah kegiatan baru..."
        autocomplete="off"
        aria-label="Input kegiatan baru"
      />
      <button type="submit" :disabled="!newTask.trim()" aria-label="Tambah kegiatan">Tambah</button>
    </form>

    <div class="filter">
      <label>
        <input type="checkbox" v-model="showOnlyPending" aria-label="Filter hanya kegiatan belum selesai" />
        kegiatan belum selesai
      </label>
    </div>

    <transition-group name="list" tag="ul" class="task-list" v-if="filteredTasks.length > 0">
      <li v-for="task in filteredTasks" :key="task.id" :class="{ done: task.done }" class="task-item">
        <label class="task-label">
          <input type="checkbox" v-model="task.done" aria-label="Checklist kegiatan" />
          <span>{{ task.text }}</span>
        </label>
        <button class="delete-btn" @click="deleteTask(task.id)" aria-label="Hapus kegiatan">&times;</button>
      </li>
    </transition-group>

    <p v-else class="empty-msg">Tidak ada kegiatan untuk ditampilkan.</p>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      newTask: '',
      tasks: [],
      showOnlyPending: false,
      nextId: 1
    }
  },
  computed: {
    filteredTasks() {
      if (this.showOnlyPending) {
        return this.tasks.filter(task => !task.done)
      }
      return this.tasks
    }
  },
  methods: {
    addTask() {
      const text = this.newTask.trim()
      if (text) {
        this.tasks.push({
          id: this.nextId++,
          text,
          done: false
        })
        this.newTask = ''
      }
    },
    deleteTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id)
    }
  }
}
</script>

<style scoped>
.todo-app {
  font-family: 'Poppins', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  color: #2c3e50; 
  background: linear-gradient(to bottom, #a8d8e8, #f0f4f8); 
  border-radius: 16px;
  padding: 24px 28px 36px;
  box-shadow:
    0 4px 8px rgba(51, 78, 104, 0.1),
    0 2px 6px rgba(118, 75, 162, 0.1);
  user-select: none;
  max-width: 400px;
  margin: 0 auto;
  transition: background-color 0.3s ease;
}

h1 {
  font-weight: 600;
  font-size: 30px;
  text-align: center;
  margin-bottom: 24px;
  letter-spacing: 0.04em;
  user-select: text;
  color: #ffffff; 
}

.input-form {
  display: flex;
  margin-bottom: 18px;
}

.input-form input[type="text"] {
  flex: 1;
  padding: 10px 14px;
  font-size: 18px;
  border: 2.5px solid #ffffff;
  border-radius: 10px 0 0 10px;
  background-color: rgba(255, 255, 255, 0.9); 
  transition: border-color 0.4s ease, background-color 0.3s ease, box-shadow 0.3s ease;
  box-shadow: inset 0 1px 4px rgba(162, 188, 226, 0.3);
}

.input-form input[type="text"]:focus {
  outline: none;
  border-color: #4db3df; 
  background-color: #e8f0ff; 
  box-shadow:
    0 0 8px 2px rgba(102, 126, 234, 0.5),
    inset 0 1px 5px rgba(102, 126, 234, 0.25);
}

.input-form button {
  background: linear-gradient(135deg, #4db3df, #5c3da0); 
  color: white;
  border: none;
  padding: 0 12px; 
  font-weight: 700;
  font-size: 16px; 
  cursor: pointer;
  border-radius: 0 10px 10px 0;
  box-shadow:
    0 5px 12px rgba(118, 75, 162, 0.55);
  transition: background 0.35s ease, transform 0.15s ease;
}

.input-form button:disabled {
  background: #a8b6db; 
  cursor: not-allowed;
  box-shadow: none;
}

.input-form button:not(:disabled):hover {
  background: linear-gradient(135deg, #4a63d8, #5c3da0);
  transform: scale(1.05);
}

.input-form button:not(:disabled):active {
  transform: scale(0.98);
}

.filter {
  font-size: 15px;
  color: #5570a1;
  margin-bottom: 18px;
  user-select: none;
  font-weight: 600;
  display: flex;
  align-items: center;
  cursor: pointer;
  transition: color 0.3s ease;
}

.filter:hover {
  color: #764ba2; 
}

.filter input[type="checkbox"] {
  margin-right: 10px;
  width: 20px;
  height: 20px;
  cursor: pointer;
  accent-color: #4db3df; 
  transition: accent-color 0.3s ease;
}

.task-label {
  display: flex;
  align-items: center; 
  cursor: pointer;
  user-select: none;
  flex: 1;
  font-weight: 500;
  font-size: 17px;
  color: #2c3e50; 
  transition: color 0.3s ease;
}

.task-label input[type="checkbox"] {
  margin-right: 10px; 
  width: 24px;
  height: 24px;
  cursor: pointer;
  accent-color: #4db3df; 
  border-radius: 5px;
  box-shadow:
    0 1px 3px rgba(67, 197, 214, 0.25);
  transition: accent-color 0.25s ease;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
  max-height: 380px;
  overflow-y: auto;
}

.task-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 16px;
  border: 1.8px solid rgba(255, 255, 255, 0.5); 
  border-radius: 12px;
  margin-bottom: 14px;
  background: rgba(255, 255, 255, 0.9);
  box-shadow:
    0 4px 10px rgba(118, 75, 162, 0.06);
  cursor: default;
  transition:
    background-color 0.25s ease,
    box-shadow 0.3s ease;
}

.task-item:hover {
  background-color: rgba(240, 242, 255, 0.8); 
  box-shadow:
    0 8px 20px rgba(63, 163, 220, 0.15);
}

.task-label {
  display: flex;
  align-items: center;
  cursor: pointer;
  user-select: none;
  flex: 1;
  font-weight: 500;
  font-size: 17px;
  color: #2c3e50;
  transition: color 0.3s ease;
}

.task-label input[type="checkbox"] {
  margin-right: 14px;
  width: 24px;
  height: 24px;
  cursor: pointer;
  accent-color: #4db3df; 
  border-radius: 5px;
  box-shadow:
    0 1px 3px rgba(67, 197, 214, 0.25);
  transition: accent-color 0.25s ease;
}

.task-list li.done .task-label span {
  text-decoration: line-through;
  color: #a1a8af; 
  font-style: italic;
  transition: color 0.3s ease;
}

.delete-btn {
  background: transparent;
  border: none;
  font-size: 28px;
  line-height: 1;
  cursor: pointer;
  color: #ff4c4c; 
  padding: 0 8px;
  transition: color 0.3s ease, transform 0.15s ease;
  user-select: none;
  font-weight: 900;
  filter: drop-shadow(0 0 2px rgba(255, 76, 76, 0.7));
  border-radius: 6px;
}

.delete-btn:hover {
  color: #d80000; 
  transform: scale(1.2);
}

.empty-msg {
  text-align: center;
  color: #ffffff; 
  font-style: italic;
  margin-top: 44px;
  user-select: none;
  font-size: 17px;
  font-weight: 500;
}


.list-enter-active,
.list-leave-active {
  transition: all 0.4s ease;
}
.list-enter-from {
  opacity: 0;
  transform: translateY(-15px);
}
.list-enter-to {
  opacity: 1;
  transform: translateY(0);
}
.list-leave-from {
  opacity: 1;
  transform: translateY(0);
}
.list-leave-to {
  opacity: 0;
  transform: translateY(20px);
}


@media (max-width: 430px) {
  .todo-app {
    box-shadow: none;
    border-radius: 0;
    max-width: 100vw;
    padding: 20px 20px 36px;
  }
}
</style>
