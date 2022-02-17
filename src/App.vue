<template>
  <div class="app-container">
    <h1 class="app-title">
      <img src="./assets/todo.jpg" alt />
      Todos
    </h1>

    <div class="composer">
      <!-- <form @submit.prevent="addTask">

        <input
          type="text"
          placeholder="Type and hit enter..."
          class="composer__input"
          @keyup="edit"
          v-model="taskName"
        />
      </form>-->

      <!-- <Composer @inputValue="inputValueHandler" @addTask="addTaskHandler"></Composer> -->

      <Composer v-model="taskName" @addTask="addTaskHandler"></Composer>

      <div class="filter">
        <!-- <div class="filter__left">
          <button
            class="filter__button filter__button--active"
            @click="filteredTasks = 'all'"
          >All ({{ todos.length }})</button>
          <button
            class="filter__button"
            @click="filteredTasks = 'pending'"
          >Pending ({{ pendingTask }})</button>
          <button class="filter__button" @click="filteredTasks = 'done'">Done ({{ doneTask }})</button>
        </div>
        <div>
          <button class="filter__button filter__button--danger" @click="clearDoneTask" v-if="todos.filter(e=>e.done).length">Clear</button>
        </div> -->
        <Filter :todos="todos" @allTasks="filteredTasks = 'all'" @pendingTasks="filteredTasks = 'pending'" @doneTasks="filteredTasks = 'done'" @clearDoneTask="clearDoneTask"></Filter>
      </div>
    </div>


    <div class="todos">
      <!-- Todo start -->

      <Todo v-for="todo in filteredTodos" :key="todo.id" :todo="todo" @onToggle="changeDoneStatus" @taskEdit="taskEditHandler"></Todo>
      <!-- <div
        class="todo"
        v-if="isTaskShow"
        v-for="todo in todos"
        :key="todo.id"
        :class="{ 'todo--done': todo.done }"
      >
        <div class="todo__status-dot"></div>
        <div @click="changeDoneStatus(todo)">
          <p class="todo__content">{{ todo.title }}</p>
          <small class="todo__time text-xs">{{ todo.time }}</small>
        </div>

        <div class="todo__edit">
          <button @click.prevent="taskEdit(todo)">📝</button>
        </div>
      </div>-->

      <!-- Todo end -->
      <div v-if="!filteredTodos.length">
        <p>Task not available</p>
      </div>
      <div >
        <p class="app-footer">developed by <span><a href="https://twitter.com/SampadSarker" >SampadSarker</a></span>  with <span class="animate-pulse">❤️</span></p>
      </div>
    </div>

    
  </div>
</template>

<script>
import Composer from './components/Composer.vue';
import Todo from './components/Todo.vue';
import Filter from './components/Filter.vue'
export default {
  name: 'App',
  components: { Todo, Composer,Filter},
  data() {
    return {
      taskName: '',
      todos: [
        // { id: 1, title: 'Javascript Learning', time: new Date().toLocaleString("en-US", { hour12: true }), done: true, edit: false },
        // { id: 2, title: 'DOM Learning', time: new Date().toLocaleString("en-US", { hour12: true }), done: false, edit: false },
        // { id: 3, title: 'Vue.js Learning', time: new Date().toLocaleString("en-US", { hour12: true }), done: true, edit: false }
      ],

      filteredTasks: 'all',
      isEdit: false,
      
      
    }
  },

  mounted() {
    //this.todos = JSON.parse(localStorage.getItem("todos"));
    //console.log("mounted--",JSON.parse(localStorage.getItem("todos")));

  },
  updated() {
    //localStorage.setItem("todos",JSON.stringify(this.todos));
  },

  

  methods: {
    taskEditHandler(todo){

      console.log("taskEditHandler.....",todo);
      

      todo.isEdit = true
      this.isEdit = true
      this.taskName = todo.title

    },

    inputValueHandler(inputValueForm) {
      this.taskName = inputValueForm
    },

    isTaskShow() {
      if (this.todos.length > 0) { return true }
      else { return false }
    },

    taskEdit(todo, event) {
      this.isEdit = true;

      todo.isEdit = true
      this.taskName = todo.title

    },

    clearDoneTask() {
      //console.log("clear clicked...........");
      
      for (let i = 0; i < this.todos.length; i++) {

        if (this.todos[i].done) {

          console.log(this.todos[i]);

          let indexOfDoneTask = this.todos.indexOf(this.todos[i])
          this.todos.splice(indexOfDoneTask, 1)  //delete


        }


      }

    },



    addTaskHandler() {

      //show alert
      if(this.taskName ==='' ){
        alert("What's your task name?")
        return   //
      }

      //new task add
      if(!this.isEdit){
        const todoName = {

        //id:this.todos[(this.todos.length)-1].id+1,
        id: Math.floor((Math.random() * 9999999999 + 1)),
        title: this.taskName,
        time: new Date().toLocaleString("en-US", { hour12: true }),
        done: false,
        isEdit: false
      }

        this.todos.unshift(todoName)  //push at beginning
        this.taskName = '' 

      }


      //edit task name
      if(this.isEdit){
        //console.log("addTask/editTask ===>")

        let length = this.todos.length

        for (let i = 0; i < length; i++) {

          if (this.todos[i].isEdit) { 

            //console.log(this.todos[i]);

            let indexOfTaskToBeEdited = this.todos.indexOf(this.todos[i])
            this.todos[indexOfTaskToBeEdited].title = this.taskName  //new task name add
            this.todos[indexOfTaskToBeEdited].isEdit = false

          }


        }

        
        this.isEdit = false
        this.taskName=''
      }
      
      
    },

    changeDoneStatus(todo) {
      todo.done = !todo.done
    },



  },
  computed: {

    filteredTodos() {
      if (this.filteredTasks == 'all') return this.todos
      else if (this.filteredTasks == 'done') return this.todos.filter(el => el.done)
      else if (this.filteredTasks == 'pending') return this.todos.filter(el => !el.done)
    },

    pendingTask() {
      let pendingTask = this.todos.filter(el => { return !el.done })
      console.log("pending", pendingTask);

      return pendingTask.length
    },

    doneTask() {
      let doneTask = this.todos.filter(el => { return el.done })
      console.log("doneTask", doneTask.length);


      return doneTask.length
    },

    

  }
}

</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
