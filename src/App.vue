<template>
  <h1>Todolist</h1>

  <button @click.prevent="hideTask">Masquer les tâches completées</button>

  <div v-if="todolist.length == 0">Vous n'avez aucune taches à faire</div>

  <form action="" @submit.prevent="addTask">
    <input type="text" placeholder="nouvelle tâche" v-model="newTask">
    <button>Ajouter Une Tâche</button>
  </form>

  <ul>
    <li v-for="task in todolist"
    :key="task.date"
    :style="{display: hide && task.completed? 'none' : ''}"
    >
        <div >
          <div :class="{completed: task.completed}">
            <h2>{{ task.title }} </h2>
            <button @click.prevent="taskCompleted(task)">[{{ task.completed? '✔' : "\u00A0\u00A0" }}]</button>
          </div>
          <h4>{{ task.date.toLocaleString() }}</h4>
        </div>
    </li>
  </ul>

</template>

<script setup>
import { ref } from "vue";

const todolist = ref([
])

const hide = ref(false)

const newTask = ref("")

const addTask = () => {
  if (!newTask.value.trim()) return

  const task = {
    date: new Date(),
    title: newTask.value,
    completed: false,
  }

  todolist.value.push(task)
  newTask.value = ''
}

const taskCompleted = (task) => {
  if (task.completed == false)
    task.completed = true
  else
    task.completed = false
}

const hideTask = () => {
  hide.value = !hide.value
}


</script>

<style>
  .completed {
    text-decoration: line-through;
  }
</style>