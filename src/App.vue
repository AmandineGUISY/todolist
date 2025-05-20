<template>
  <div class="todo-wrapper">
    <div class="todolist">
      <h1>Todolist</h1>
      <button class="eye" @click.prevent="hideTask">
        {{ hide ? '🙈' : '👁️' }}
      </button>
    </div>

    <div v-if="todolist.length === 0" class="empty">Vous n'avez aucune tâche à faire</div>

    <form @submit.prevent="addTask" class="form">
      <input type="text" placeholder="Nouvelle tâche" v-model="newTask" />
      <button class="add">+</button>
    </form>

    <ul class="task-list">
      <li
        v-for="task in todolist"
        :key="task.date"
        :style="{ display: hide && task.completed ? 'none' : '' }"
        class="task-item"
      >
        <div class="task-content">
          <div :class="{ completed: task.completed }" class="task-title">
            <h2>{{ task.title }}</h2>
            <button @click.prevent="taskCompleted(task)">
              {{ task.completed ? '✔' : "\u00A0\u00A0" }}
            </button>
          </div>
          <h4>{{ task.date.toLocaleString() }}</h4>
          <button @click="deleteTask(task)" class="delete">X</button>
        </div>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from "vue";

const todolist = ref([]);
const hide = ref(false);
const newTask = ref("");

const addTask = () => {
  if (!newTask.value.trim()) return;

  const task = {
    date: new Date(),
    title: newTask.value,
    completed: false,
  };

  todolist.value.push(task);
  newTask.value = "";
};

const taskCompleted = (task) => {
  task.completed = !task.completed;
};

const hideTask = () => {
  hide.value = !hide.value;
};

const deleteTask = (task) => {
  todolist.value = todolist.value.filter((t) => t !== task);
};
</script>

<style>
body {
  background-color: bisque;
  display: flex;
  justify-content: center;
  padding: 2rem;
  margin: 0;
  font-family: sans-serif;
  height: 100vh;
}

.todo-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  max-width: 900px;
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  padding: 1rem;
}

.todolist {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  max-width: 600px;
  margin-bottom: 1rem;
}

h1 {
  color: rgb(194, 108, 51);
  margin: 0;
}

.eye {
  font-size: 1.5rem;
  background: none;
  border: none;
  cursor: pointer;
}

.empty {
  margin-bottom: 1rem;
  font-style: italic;
}

.form {
  display: flex;
  gap: 10px;
  margin-bottom: 1rem;
  max-width: 600px;
  width: 100%;
}

input[type="text"] {
  flex-grow: 1;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 0.5rem;
}

.add {
  padding: 0 1rem;
  background-color: rgb(255, 168, 168);
  color: white;
  border: none;
  border-radius: 0.5rem;
  cursor: pointer;
}

.add:hover {
  background-color: rgb(255, 120, 120);
}

.task-list {
  list-style: none;
  padding: 0;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
  max-width: 900px;
  width: 100%;
  flex-grow: 1;
  overflow-y: auto;
  min-height: 0;
}

.task-item {
  background: white;
  border-radius: 0.5rem;
  padding: 1rem;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  height: 200px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.task-content {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.task-title {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.completed h2 {
  text-decoration: line-through;
  color: gray;
}

.delete {
  align-self: flex-end;
  background-color: rgb(255, 100, 100);
  color: white;
  border: none;
  border-radius: 0.5rem;
  padding: 0.3rem 0.7rem;
  cursor: pointer;
}

.delete:hover {
  background-color: rgb(220, 70, 70);
}

.task-list li {
  background-color: #fff8f0;
  padding: 1em;
  border-radius: 10px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  word-wrap: break-word;
  overflow-wrap: break-word;
  max-width: 100%;
}

.task-title h2 {
  font-size: 1.1rem;
  margin: 0;
  word-break: break-word;
  white-space: normal;
}
</style>
