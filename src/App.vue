<template>
  <div id="app" class="container">
    <HeaderComponent />
    <div class="mb-4">
      <h3 class="text-left">Add items</h3>
      <p
        class="text-success text-left"
        v-if="message"
        v-text="message"
      />
      <TaskEditComponent
        v-if="editedTask"
        v-model="editedTaskTitle"
        @save="storeTask"
        @cancel="cancelEditTask"
      />
      <TaskAddComponent
        v-else
        v-model="newTask"
        @save="storeTask"
       />
    </div>

    <TaskListComponent
      :tasks="tasks"
      @edit="startEditTask"
      @delete="deleteTask"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import HeaderComponent from './components/HeaderComponent.vue';
import TaskListComponent from './components/TaskListComponent.vue';
import { task as taskModel } from './models/task';
import TaskAddComponent from './components/TaskAddComponent.vue';
import TaskEditComponent from './components/TaskEditComponent.vue';

const tasks = ref<taskModel[]>([]);
const editedTask = ref<taskModel | null>(null);
const editedTaskTitle = ref("");
const newTask = ref("");
const message = ref("");
let lastId = 0;

onMounted(() => {
  const jsonItems = localStorage.getItem("items");
  if (jsonItems) {
    const items = JSON.parse(jsonItems);
    if (items) {
      tasks.value = items;
      lastId = tasks.value.reduce((prev, current) => {
        return prev < current.id ? current.id : prev;
      }, 0)
    }
  }
});

function store() {
  localStorage.setItem("items", JSON.stringify(tasks.value));
}
function startEditTask(task: taskModel) {
  message.value = "";
  editedTask.value = task
  editedTaskTitle.value = task.title;
}
function cancelEditTask() {
  editedTask.value = null;
}
function storeTask() {
  if (editedTask.value) {
    // modify task
    editedTask.value.title = editedTaskTitle.value;
    message.value = "Text edited successfully";
    editedTask.value = null;
  } else {
    // create new task
    lastId++;
    tasks.value.push({
      id: lastId,
      title: newTask.value
    });
    newTask.value = "";
    message.value = "";
  }
  store();
}
function deleteTask(task: taskModel) {
  message.value = "";
  const index = tasks.value.indexOf(task);
  tasks.value.splice(index, 1);
  store();
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
