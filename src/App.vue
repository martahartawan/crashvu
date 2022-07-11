<template>
  <div class="container">
    <Header
      @toggle-add-task="toggleAddTask"
      title="Task Tracker"
      :showAddTask="showAddTask"
    />
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks
      @toggle-reminder="toggleReminder"
      @delete-task="deleteTask"
      :tasks="tasks"
    />
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "./components/Header";
import Tasks from "./components/Tasks";
import AddTask from "./components/AddTask";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
      showAddTask: false,
    };
  },
  methods: {
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    async addTask(task) {
      const res = await fetch(
        "https://6283bcbc38279cef71dbafbe.mockapi.io/vuejs",
        {
          method: "POST",
          headers: {
            "Content-Type": "aplication/json",
            body: JSON.stringify(task),
          },
        }
      );

      const data = await res.json();
      this.tasks = [...this.tasks, task];
    },
    async deleteTask(id) {
      if (confirm("apakah anda ingin menghapus ?")) {
        const res = await fetch(
          "https://6283bcbc38279cef71dbafbe.mockapi.io/vuejs/${id}",
          {
            method: "DELETE",
          }
        );
        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("Error delete")
      }
    },
    toggleReminder(id) {
      this.tasks = this.tasks.map(
        task => (task.id === id ? { ...task, reminder: !task.reminder } : task)
      );
    },
    async fetchTasks() {
      const res = await fetch(
        "https://6283bcbc38279cef71dbafbe.mockapi.io/vuejs"
      );
      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      const res = await fetch(
        "https://6283bcbc38279cef71dbafbe.mockapi.io/vuejs/${id}"
      );
      const data = await res.json();
      return data;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
