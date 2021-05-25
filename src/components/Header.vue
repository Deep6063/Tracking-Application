<template> 
    <header> 
        <h1>{{ title }}</h1>
        <Button @btn-click ="$emit('toggle-add-task')" :text="closeAddTask ? 'Close' : 'Add Task'" :color ="closeAddTask ? 'red' : 'green'"/>
    </header>
</template>

<script> 
import Button from './Button.vue'

export default {
    name: 'Header',
    props: {
        title : String,
        closeAddTask: Boolean, 
    },
    components: {
        Button
    }
}
</script> 

<style scoped> 
    header {
        display: flex;  
        justify-content: space-between; 
        align-items: center; 
        margin-bottom: 20px;
    }
    
</style>

