<script setup>
import { ref, computed } from "vue"
import List from "./List.vue"

const tasks = ref([
  {
    id: 1,
    title: "Eat breakfast",
    status: "PENDING"
  },
  {
    id: 2,
    title: "Eat lunch",
    status: "PENDING"
  },
  {
    id: 3,
    title: "Coding",
    status: "COMPLETED"
  },
  {
    id: 4,
    title: "Eat dinner",
    status: "PENDING"
  }
])

let new_task_title = ref("")

const pendingTasks = computed(() => tasks.value.filter(el => el.status == "PENDING"))
const completedTasks = computed(() => tasks.value.filter(el => el.status == "COMPLETED"))
let message = ref("")

function delete_item(id) {
  tasks.value.filter((el) => el.id == id)[0].status = "DELETED"
}

function change_task_status(id) {
  const task = tasks.value.find((el) => el.id == id)
  if (task) {
    task.status = task.status == "PENDING" ? "COMPLETED" : "PENDING";
  }
}

function add_task() {
  if (new_task_title.value.trim() != "") {
    if (new_task_title.value.length <= 50) {
      tasks.value.push({
        id: tasks.value.length + 1,
        title: new_task_title.value,
        status: "PENDING"
      })
      message.value = "Task added successfully"
      new_task_title.value = ""
    } else {
      message.value = "Task name cannot have more than 50 characters"
    }
  } else {
    message.value = "Task name cannot be empty"
  }

  setTimeout(() => {
    message.value = ""
  }, 2000);
}

</script>
<template>
<header>
  <h1>TODO Website</h1>
  <span>Create a new TODO task</span>
  <div>
    <input type="text" v-model="new_task_title" placeholder="Enter task name"/>
    <button class="createbutton" @click="add_task()">Create</button>
  </div>
</header>
<main>
  <div class="text2">{{ message }}</div>
  <List :pendingTasks="pendingTasks"
  :delete_item="delete_item"
  :change_task_status="change_task_status"
  :completedTasks="completedTasks"/>
  <ul class="completed_tasks" v-if="completedTasks.length > 0">
    <li v-for="item in completedTasks" :key="item.id">
      <span>
        <input type="checkbox" @change="change_task_status(item.id)" v-model="item.checked" :checked="item.status=='COMPLETED'"/>
        <span>{{ item.title }}</span>
      </span>
      <div>
        <button @click="delete_item(item.id)"><i class="button1"></i></button>
      </div>
    </li>
  </ul>
</main>
</template>