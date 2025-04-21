<template>
    <ul class="pending_tasks" v-if="pendingTasks.length > 0">
    <li v-for="item in pendingTasks">
      <span>
        <input type="checkbox" @change="change_task_status(item.id)" v-model="item.checked"/>
        <span>{{ item.title }}</span>
      </span>
      <div>
        <button @click="delete_item(item.id)"><i class="button1"></i></button>
      </div>
    </li>
  </ul>
  <div class="text1" v-else-if="completedTasks.length > 0">
    All tasks has been completed!
  </div>
  <div class="text2" v-else>
    There are currently no tasks
  </div>
</template>

<script setup>
defineProps(["pendingTasks", "delete_item", "change_task_status", "completedTasks"])
</script>