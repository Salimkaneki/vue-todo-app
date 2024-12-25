<template>
  <div class="task-manager">
    <h1>Gestionnaire de tâches</h1>
    <p v-if="tasks.length === 0" class="no-tasks">Vous n'avez pas encore de tâches.</p>

    <!-- Formulaire pour ajouter une tâche -->
    <form @submit.prevent="addTask" class="task-form">
      <input 
        type="text" 
        placeholder="Ajoutez une nouvelle tâche..." 
        v-model="newTaskTitle" 
      />
      <button>Ajouter</button>
    </form>

    <!-- Option de filtre -->
    <div v-if="tasks.length > 0" class="filter">
      <label>
        <input type="checkbox" v-model="hideCompleted" />
        Masquer les tâches terminées
      </label>
    </div>

    <!-- Liste des tâches -->
    <ul class="task-list">
      <li 
        v-for="(task, index) in filteredTasks" 
        :key="index"
        :class="{ completed: task.completed }"
      >
        <label class="task-item">
          <input 
            type="checkbox" 
            v-model="task.completed" 
          />
          <span>{{ task.title }}</span>
        </label>
        <button @click="deleteTask(index)">🗑</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const tasks = ref([]);
const newTaskTitle = ref('');
const hideCompleted = ref(false);

const addTask = () => {
  if (newTaskTitle.value.trim() === '') {
    alert('Veuillez entrer une tâche valide.');
    return;
  }

  tasks.value.push({
    title: newTaskTitle.value.trim(),
    completed: false,
  });

  newTaskTitle.value = '';
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

const filteredTasks = computed(() => {
  const sortedTasks = [...tasks.value].sort((a, b) => a.completed - b.completed);
  return hideCompleted.value
    ? sortedTasks.filter(task => !task.completed)
    : sortedTasks;
});
</script>

<style scoped>
/* Conteneur principal */
.task-manager {
  max-width: 600px;
  margin: 40px auto;
  padding: 30px;
  border-radius: 20px;
  background: linear-gradient(135deg, #e3fdfd, #ffe6fa);
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  font-family: 'Poppins', sans-serif;
  text-align: center;
}

/* Titre */
.task-manager h1 {
  font-size: 2.5rem;
  color: #4a4e69;
  margin-bottom: 20px;
}

/* Message si aucune tâche */
.no-tasks {
  font-size: 1.2rem;
  color: #9a8c98;
  margin-bottom: 20px;
}

/* Formulaire */
.task-form {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 20px;
}

.task-form input {
  flex: 1;
  padding: 15px;
  font-size: 1rem;
  border: 2px solid #c9ada7;
  border-radius: 25px;
  outline: none;
  transition: all 0.3s ease;
}

.task-form input:focus {
  border-color: #4a4e69;
  box-shadow: 0 0 10px rgba(74, 78, 105, 0.5);
}

.task-form button {
  padding: 12px 20px;
  font-size: 1rem;
  background-color: #4a4e69;
  color: white;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  transition: transform 0.2s ease, background-color 0.3s ease;
}

.task-form button:hover {
  background-color: #22223b;
  transform: scale(1.1);
}

/* Filtre */
.filter {
  margin-bottom: 20px;
  font-size: 1rem;
  color: #4a4e69;
}

.filter label {
  display: inline-flex;
  align-items: center;
  gap: 5px;
}

/* Liste des tâches */
.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.task-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 15px 20px;
  border: 1px solid #dcdcdc;
  border-radius: 15px;
  margin-bottom: 10px;
  background-color: #ffffff;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s ease, background-color 0.3s ease;
}

.task-list li:hover {
  transform: translateY(-3px);
  background-color: #f8f8f8;
}

/* Tâches terminées */
.task-list li.completed span {
  text-decoration: line-through;
  color: #9a8c98;
}

.task-item {
  display: flex;
  align-items: center;
  gap: 10px;
}

.task-item input[type="checkbox"] {
  width: 18px;
  height: 18px;
}

.task-item span {
  font-size: 1.1rem;
  color: #4a4e69;
}

/* Bouton supprimer */
.task-list li button {
  padding: 8px 12px;
  font-size: 1rem;
  color: white;
  background-color: #c44536;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: transform 0.2s ease, background-color 0.3s ease;
}

.task-list li button:hover {
  background-color: #a12722;
  transform: scale(1.1);
}
</style>
