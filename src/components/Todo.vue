<template>
  <div
        class="todo"
        
        
        :class="{ 'todo--done': todo.done }"
      >
        <div class="todo__status-dot"></div>
        <div @click="$emit('onToggle',todo)" >
          <p class="todo__content">{{ todo.title }}</p>
          <small class="todo__time text-xs">{{ todo.time }}</small>
        </div>

        <div class="todo__edit">
          <button @click.prevent="taskEdit(todo)">📝</button>
        </div>
      </div>
</template>

<script>
export default {
    name:'Todo',
    props:['todo'],
    emits:['onToggle'],

}
</script>

<style>

</style>