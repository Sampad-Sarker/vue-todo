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

      <Todo v-for="todo in filteredTodos" :key="todo.id" :todo="todo" @onToggle="changeDoneStatus"></Todo>
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
      isNewTask: true,
      
    }
  },

  mounted() {
    //[this.todos] = [JSON.parse(localStorage.getItem("todos"))];
    //console.log("mounted--",JSON.parse(localStorage.getItem("todos")));

  },
  updated() {
    //localStorage.setItem("todos",JSON.stringify(this.todos));
  },

  watch: {
    taskName(newValue, oldValue) {
      if (this.isEdit) {
        console.log("newValue", newValue, "oldValue", oldValue);

        // this.todos.filter((el) => {
        //   if (el.edit) { el.edit==false; return el.title = newValue ;}
        // })

        this.todos.filter(el => el.edit).title = newValue

      }
    }
  },

  methods: {
    inputValueHandler(inputValueForm) {
      this.taskName = inputValueForm
    },

    isTaskShow() {
      if (this.todos.length > 0) { return true }
      else { return false }
    },

    // edit(e) {
    //   console.log('edit....', e.target.value);
    //   let editedTaskName = e.target.value

    //   if (this.isEdit) {
    //     console.log("eeeeeeeeeeeeeeeeeeeeeeeeeee");

    //     this.todos.filter((el) => {
    //       if (el.edit) { el.title = editedTaskName }
    //     })

    //   }

    // },

    taskEdit(todo, event) {
      this.isEdit = true;
      this.isNewTask = false
      todo.edit = true
      this.taskName = todo.title




      // let editedTaskName = event.target.value

      // todo.title = editedTaskName
      // todo.edit = false


      // console.log('taskEdit:==>',todo,)

    },

    clearDoneTask() {
      console.log("clear clicked...........");
      //let doneTask = this.todos.filter(el=>{return el.done})
      //console.log(doneTask);


      //console.log("clear clicked",this.doneTask);
      // this.todos.filter(el => {return el.done}).splice(1,1)




      for (let i = 0; i < this.todos.length; i++) {

        if (this.todos[i].done) {

          console.log(this.todos[i]);

          let indexOfDoneTask = this.todos.indexOf(this.todos[i])
          this.todos.splice(indexOfDoneTask, 1)


        }


      }

    },



    addTaskHandler(e) {

      // if (e == '') {
      //   alert("what's your Task Name?")
      //   return
      // }
      const todoName = {

        //id:this.todos[(this.todos.length)-1].id+1,
        id: Math.floor((Math.random() * 1000000 + 1)),
        title: this.taskName,
        time: new Date().toLocaleString("en-US", { hour12: true }),
        done: false,
        isEdit: false
      }

      this.todos.unshift(todoName)

      this.taskName = ''

      // if (this.isEdit) {

      //   this.isEdit = false
      //   this.isNewTask = false
      //   this.taskName = ''

      //   console.log("XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX.....");


      // }

      // if (e.target.value == '') {
      //   alert("what's your Task Name?")

      // }

      // if (this.isNewTask && !this.isEdit) {
      //   this.todos.push({

      //     //id:this.todos[(this.todos.length)-1].id+1,
      //     id: Math.floor((Math.random() * 1000000 + 1)),
      //     title: this.taskName,
      //     time: new Date().toLocaleString("en-US", { hour12: true }),
      //     done: false,
      //     isEdit: false
      //   })
      // }
      // this.taskName = ""

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

    // clearDoneTask(){
    //   console.log("clear clicked");

    // }

    // clearDoneTask(){
    //   // let doneTask = this.todos.filter(el=>{return el.done})


    //   console.log("clear");

    //   // for (let i = 0; i < this.todos.length; i++) {
    //   //   if(this.todos[i].done){
    //   //     delete this.todos[i]
    //   //   } 
    //   // }

    // }


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
