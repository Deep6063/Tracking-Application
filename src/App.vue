<template>
  <div class="container"> 
    <Header @toggle-add-task="toggleAddTask"  title="Task Tracker" :closeAddTask= "showAddTask"/>

    <div v-if="showAddTask">
      <AddTask @add-task="addTask"/>
    </div>
    
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"/> 
    <Footer />
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Tasks from './components/Tasks.vue'
import AddTask from './components/AddTask'
import Footer from './components/Footer'

export default {
  name: 'App',
  components: {
    Header, 
    Tasks,
    AddTask, 
    Footer,
  }, 
  data() {
    return {
      tasks: [], 
      showAddTask: false,
    }
  }, 

  methods: {

    toggleAddTask() {
      this.showAddTask = !this.showAddTask
    },

    async addTask(task) {
      const res = await fetch('http://localhost:5000/tasks', {
        method: 'POST', 
        headers: {
          'Content-type': 'application/json', 
        }, 
        body: JSON.stringify(task)
      })
      console.log(res)
      const data = await res.json()
      
      this.tasks = [...this.tasks, data]
    },

    async deleteTask(id) {
      if(confirm('Are you Sure?')){
        const res = await fetch(`http://localhost:5000/tasks/${id}`, {method: 'DELETE'})
        res.status == 200? (this.tasks = this.tasks.filter( (task) => task.id != id))  : alert('Error Deleting Task')
      }  
    },

    async toggleReminder(id) {
      const taskToggle = await this.fetchTask(id)
      const updTask = {...taskToggle, reminder: !taskToggle.reminder}
    
      const res = await fetch(`http://localhost:5000/tasks/${id}`,{
        method: 'PUT', 
        headers: {
          'Content-type': 'application/json'
        }, 
        body: JSON.stringify(updTask)
      })
        
      const data = await res.json()

      this.tasks = this.tasks.map((task)=> task.id === id ? {...task, reminder: data.reminder} : task)
    }, 

    async fetchTasks() {
      const res = await fetch('http://localhost:5000/tasks')
      const data = await res.json()
      return data
    }, 

    async fetchTask(id) {
      const res = await fetch(`http://localhost:5000/tasks/${id}`)
      const data = await res.json()
      return data
    }
  },

  async created() {
    this.tasks = await this.fetchTasks()
  }
}
</script>

<style>

.container {
  max-width: 500px; 
  min-height: 500px;
  border: 2px solid black; 
  overflow: auto; 
  padding: 30px;
  margin: 30px auto;

}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: left;
  color: #2c3e50;
  margin-top: 60px;
}
.btn {
  display: inline-block; 
  background: #000000; 
  color: #FFF; 
  border: 1px #ff00ff; 
  border-radius: 5px;
  cursor: pointer; 
    }
</style>
