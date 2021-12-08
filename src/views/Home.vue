<template>
  <AddTask v-if="showAddTask" @add-task="addTask" />
  <p v-else>Click on AddTask button to add a task</p>
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from "../components/Tasks.vue";
import AddTask from "../components/AddTask.vue";

export default {
  name: "App",
  components: {
    Tasks,
    AddTask,
  },
  props:{
      showAddTask: Boolean
  },
  data() {
      return {
          tasks: [],
      }
  },
  async created() {
    // static
    // this.tasks = [
    //   {
    //     id: 1,
    //     text: "Doctor appointment",
    //     reminder: true,
    //   },
    //   {
    //     id: 2,
    //     text: "Lawyer appointment",
    //     reminder: false,
    //   },
    // ];

    const result = await fetch("http://127.0.0.1:5000/gettasks");
    const data = await result.json();
    this.tasks = result.status == 200 && data ? data : this.tasks;
  },
  methods: {
      async deleteTask(id) {
      if (confirm("Are you sure?")) {
        // static
        // this.tasks = this.tasks.filter((task) => task.id != id);

        const requestOptions = {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ id: id }),
        };
        const result = await fetch(
          "http://127.0.0.1:5000/deletetask",
          requestOptions
        );
        const data = result.status == 200 && (await result.json());

        data ? (this.tasks = data) : alert("Failed to delete task.");
      }
    },
    async toggleReminder(id) {
      // static
      // this.tasks = this.tasks.map((task) =>
      //   task.id === id ? { ...task, reminder: !task.reminder } : task
      // );

      const requestOptions = {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ id: id }),
      };
      const result = await fetch(
        "http://127.0.0.1:5000/togglereminder",
        requestOptions
      );
      const data = result.status == 200 && (await result.json());

      data ? (this.tasks = data) : alert("Failed to toggle reminder.");
    },
    async addTask(task) {
      // static
      //this.tasks = [...this.tasks, task];

      const requestOptions = {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ task: task }),
      };
      const result = await fetch(
        "http://127.0.0.1:5000/addtask",
        requestOptions
      );
      const data = result.status == 200 && (await result.json());

      data ? (this.tasks = data) : alert("Failed to add task.");
    },
  }
};
</script>