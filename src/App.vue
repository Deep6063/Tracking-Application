<template>
  <div class="container"> 
    <Header title="Task Tracker" />
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"/> 
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Tasks from './components/Tasks.vue'

export default {
  name: 'App',
  components: {
    Header, 
    Tasks,
  }, 
  data() {
    return {
      tasks: []
    }
  }, 
  methods: {
    deleteTask(id) {
      if(confirm('Are you Sure?')){
        this.tasks = this.tasks.filter((task) => task.id != id)
      }
      
    },
    toggleReminder(id) {
        console.log(id)
    }
  },
  created() {
    this.tasks = [
      {
        id: 1, 
        title: 'Doctors Appointment', 
        day: 'May 29th at 2:20 pm', 
        reminder: true,
      },
      {
        id: 2, 
        title: 'Zoom Meeting', 
        day: 'June 1st at 11:20 am', 
        reminder: true,
      },
      {
        id: 3, 
        title: 'ICBC Driving Test', 
        day: 'January 10th 2022 at 11:20 am', 
        reminder: false,
      }
    ]
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
  text-align: center;
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
