<template>
  <main>
    <header>
      <h1>Pinia Tasks</h1>
    </header>
    <!-- new task form  -->
    <div class="new-task-form">
      <TaskForm />
    </div>

    <!-- filter  -->
    <nav class="filter">
      <button @click="filter = 'all'">All tasks</button>
      <button @click="filter = 'favs'">Favs tasks</button>
    </nav>

    <!-- loading -->
    <div class="loading" v-if="loading">Loading tasks...</div>

    <!-- task list -->
    <div class="task-list" v-if="filter === 'all'">
      <p>You have {{ totalCount }} tasks left</p>
      <div v-for="task in tasks">
        <TaskDetails :task="task" />
      </div>
    </div>
    <div class="task-list" v-if="filter === 'favs'">
      <p>You have {{ favCount }} Tasks</p>
      <div v-for="task in favs">
        <TaskDetails :task="task" />
      </div>
    </div>

    <button @click="taskStore.$reset">RESET STATE</button>
  </main>
</template> 

<script>
import { useTaskStore } from "./store/taskStore";
import TaskDetails from "./components/taskDetails.vue";
import { ref } from "vue";
import TaskForm from "./components/taskForm.vue";
import { storeToRefs } from "pinia";

export default {
  components: { TaskDetails, TaskForm },
  setup() {
    const taskStore = useTaskStore();

    const { tasks, loading, favs, totalCount, favCount } =
      storeToRefs(taskStore);

    // fetch //
    taskStore.getTasks();

    const filter = ref("all");

    return { taskStore, filter, tasks, loading, favs, totalCount, favCount };
  },
};
</script>