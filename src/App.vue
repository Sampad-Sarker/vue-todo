<template>
  
  <div class="app-container">
  
  <h1 class="app-title">
    <img src="./assets/todo.jpg" alt="" >
    Todos
  </h1>

  <div class="composer">
    <input type="text" 
      placeholder="Type and hit enter..." 
      maxlength="20"
      class="composer__input" 
      @keypress.enter = "addTask"
      v-model="taskName"
      
    />
    
    <div class="filter">
      <div class="filter__left">
        <button class="filter__button filter__button--active">All ({{todos.length}})</button>
        <button class="filter__button">Pending ({{pendingTask}})</button>
        <button class="filter__button">Done ({{doneTask}})</button>
      </div>
      <div >
        <button class="filter__button filter__button--danger" @click="clearDoneTask">Clear</button>
      </div>
    </div>
  </div>

  <div class="todos">
    <!-- Todo start -->
    <div class="todo" 
      v-for="todo in todos" 
      :key="todo.id" 
      :class="{'todo--done':todo.done}"
      
      @click="changeDoneStatus(todo)"
    >
      <div class="todo__status-dot" ></div>
      <div>
        <p class="todo__content">{{todo.title}}</p>
        <small class="todo__time">{{todo.time}}</small>
      </div>
      
      <!-- <div class="todo__edit">
        <p @click="taskEdit">📝</p>
      </div> -->
      
    </div>
    <!-- Todo end -->

    <!-- Todo start -->
    <!-- <div class="todo">
      <div class="todo__status-dot"></div>
      <div>
        <p class="todo__content">sdfsa</p>
        <small class="todo__time"> Wed Feb 09 2022 21:19:25 GMT+0600 (Bangladesh Standard Time) </small>
      </div>
    </div> -->
    <!-- Todo end -->

    <!-- Todo start -->
    <!-- <div class="todo todo--done">
      <div class="todo__status-dot"></div>
      <div>
        <p class="todo__content">sdfsa</p>
        <small class="todo__time"> Wed Feb 09 2022 21:19:25 GMT+0600 (Bangladesh Standard Time) </small>
      </div>
    </div> -->
    <!-- Todo end -->
  </div>
</div>

  
</template>

<script>
export default{
  name:'App',
  
  data() {
    return {
      taskName:'',
      todos:[
        {id:1,title:'Javascript Learning',time:new Date().toLocaleString("en-US",{hour12: true}),done:true},
        {id:2,title:'DOM Learning',time:new Date().toLocaleString("en-US",{ hour12: true }),done:false},
        {id:3,title:'Vue.js Learning',time:new Date().toLocaleString("en-US",{ hour12: true }),done:true}
      ],
      activated:"clear-button-activated",
      deactivated:"clear-button-deactivated",
    }
  },

  mounted() {
    //[this.todos] = [JSON.parse(localStorage.getItem("todos"))];
    //console.log("mounted--",JSON.parse(localStorage.getItem("todos")));
    
  },
  updated() {
    localStorage.setItem("todos",JSON.stringify(this.todos));
  },

  methods: {
    // taskEdit(){

    // },

    clearDoneTask(){
      console.log("clear clicked...........");
      //let doneTask = this.todos.filter(el=>{return el.done})
      //console.log(doneTask);
      
      
      //console.log("clear clicked",this.doneTask);
      // this.todos.filter(el => {return el.done}).splice(1,1)
      
      
      
      
      for (let i = 0; i < this.todos.length; i++) {

        if(this.todos[i].done){

          console.log(this.todos[i]);

          let indexOfDoneTask = this.todos.indexOf(this.todos[i])
          this.todos.splice(indexOfDoneTask,1)
          
          
        } 
        
        
      }
      
    },



    addTask(){
      this.todos.push({
        
        //id:this.todos[(this.todos.length)-1].id+1,
        id:Math.floor((Math.random()*1000000+1)),
        title:this.taskName,
        time:new Date().toLocaleString("en-US",{hour12: true}),
        done:false
      })

      this.taskName = ""
    },

    changeDoneStatus(todo){
      todo.done = !todo.done
    },

    
    
  },
  computed:{
    pendingTask(){
      let pendingTask = this.todos.filter(el=>{return !el.done})
      console.log("pending",pendingTask);
      
      return pendingTask.length
    },

    doneTask(){
      let doneTask = this.todos.filter(el=>{return el.done})
      console.log("doneTask",doneTask.length);
      

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
