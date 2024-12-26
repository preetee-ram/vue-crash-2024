<script>
//composition API with explicit setup()
import { ref } from "vue";
export default {
  setup() {
    const name = ref("John Doe");
    const status = ref("active");
    const tasks = ref(["Task 1", "Task 2", "Task 3"]);

    const toggleStatus = () => {
      if (status.value === "active") {
        status.value = "pending";
      } else if (status.value === "pending") {
        status.value = "inactive";
      } else {
        status.value = "active";
      }
    };
    return {
      name,
      status,
      tasks,
      toggleStatus,
    };
  },
};
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">User is active</p>
  <p v-else-if="status === 'pending'">user is pending</p>
  <p v-else>user is inactive</p>

  <h3>Tasks:</h3>
  <ul>
    <li v-for="task in tasks" :key="task">{{ task }}</li>
  </ul>

  <br />
  <button @click="toggleStatus">Change status</button>
</template>

<style scoped></style>
