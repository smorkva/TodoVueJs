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

<script lang="ts">
import { Component, Emit, Vue } from 'vue-property-decorator';
import HeaderComponent from './components/HeaderComponent.vue';
import TaskListComponent from './components/TaskListComponent.vue';
import { task as taskModel } from './models/task';
import TaskAddComponent from './components/TaskAddComponent.vue';
import TaskEditComponent from './components/TaskEditComponent.vue';

@Component({
  components: {
    HeaderComponent,
    TaskListComponent,
    TaskAddComponent,
    TaskEditComponent,
  },
})

export default class App extends Vue {
  tasks: taskModel[] = [];
  editedTask: taskModel | null = null;
  editedTaskTitle = "";
  newTask = "";
  message = "";
  lastId = 0;

  mounted() {
    const jsonItems = localStorage.getItem("items");
    if (jsonItems) {
      const tasks = JSON.parse(jsonItems);
      if (tasks) {
        this.tasks = tasks;
        this.lastId = this.tasks.reduce((prev, current) => {
          return prev < current.id ? current.id : prev;
        }, 0)
      }
    }
  }

  @Emit() store() {
    localStorage.setItem("items", JSON.stringify(this.tasks));
  }
  @Emit() startEditTask(task: taskModel) {
    this.message = "";
    this.editedTask = task
    this.editedTaskTitle = task.title;
  }
  @Emit() cancelEditTask() {
    this.editedTask = null;
  }
  @Emit() storeTask() {
    if (this.editedTask) {
      // modify task
      this.editedTask.title = this.editedTaskTitle;
      this.message = "Text edited successfully";
      this.editedTask = null;
    } else {
      // create new task
      this.lastId++;
      this.tasks.push({
        id: this.lastId,
        title: this.newTask
      });
      this.newTask = "";
      this.message = "";
    }
    this.store();
  }
  @Emit() deleteTask(task: taskModel) {
    this.message = "";
    const index = this.tasks.indexOf(task);
    this.tasks.splice(index, 1);
    this.store();
  }
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
