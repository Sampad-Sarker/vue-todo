<template>
    <div class="filter__left">
        <button
            class="filter__button filter__button--active"
            @click="$emit('allTasks')"
            
        >All ({{ todos.length }})</button>
        <button
            class="filter__button"
            @click="$emit('pendingTasks')"
            v-if="pendingTask"
        >Pending ({{ pendingTask }})</button>
        <button class="filter__button" 
            @click="$emit('doneTasks')"
            v-if="doneTask"
        >Done ({{ doneTask }})</button>
    </div>
    <div>
        <button
            class="filter__button filter__button--danger"
            @click="$emit('clearDoneTask')"
            
            v-if="doneTask"
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