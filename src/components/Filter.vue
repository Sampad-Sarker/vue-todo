<template>
    <div class="filter__left">
        <button
            class="filter__button filter__button--active"
            @click="$emit('allTasks')"
            
        >All ({{ todos.length }})</button>
        <button
            class="filter__button"
            @click="$emit('pendingTasks')"
            
        >Pending ({{ pendingTask }})</button>
        <button class="filter__button" 
            @click="$emit('doneTasks')"
            
        >Done ({{ doneTask }})</button>
    </div>
    <div>
        <button
            class="filter__button filter__button--danger"
            @click="$emit('clearDoneTask')"
            
            v-if="todos.filter(e => e.done).length"
        >Clear</button>
    </div>
</template>

<script>
export default {
    name: 'Filter',
    props:['todos'],
    emits:['allTasks','doneTasks','pendingTasks','clearDoneTask'],

    data() {
        return {
            filteredTasks : 'all'
        }
    },

    computed:{
        pendingTask(){

            return this.todos.filter(todo=>!todo.done).length

        },
        doneTask(){
            return this.todos.filter(todo=>todo.done).length
        }
    }
}
</script>

<style>
</style>