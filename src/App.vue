<template>
  
  <div class="app-container">
  
  <h1 class="app-title">
    <img src="./assets/todo.jpg" alt="" >
    Todos
  </h1>

  <div class="composer">
    <input type="text" 
      placeholder="Type and hit enter..." 
      
      class="composer__input" 
      @keypress.enter = "addTask"
      @keyup="edit"
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
    <div class="todo" v-if="isTaskShow"
      v-for="(todo,index) in todos" 
      :key="todo.id" 
      :class="{'todo--done':todo.done}"
      
      
    >
      <div class="todo__status-dot" ></div>
      <div @click="changeDoneStatus(todo)">
        
        <p class="todo__content" >{{todo.title}}</p>
        <small class="todo__time text-xs">{{todo.time}}</small>
      </div>
      
      <div class="todo__edit">
        <button @click="taskEdit(todo,index)" 
            >📝</button>
      </div>
      
    </div>

    <!-- Todo end -->
    <div v-if="isTaskShow">
      <p>Task not available</p>
    </div>
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
        {id:1,title:'Javascript Learning',time:new Date().toLocaleString("en-US",{hour12: true}),done:true,edit:false},
        {id:2,title:'DOM Learning',time:new Date().toLocaleString("en-US",{ hour12: true }),done:false,edit:false},
        {id:3,title:'Vue.js Learning',time:new Date().toLocaleString("en-US",{ hour12: true }),done:true,edit:false}
      ],
      isEdit:false,
      activated:"clear-button-activated",
      deactivated:"clear-button-deactivated",
    }
  },

  mounted() {
    //[this.todos] = [JSON.parse(localStorage.getItem("todos"))];
    //console.log("mounted--",JSON.parse(localStorage.getItem("todos")));
    
  },
  updated() {
    //localStorage.setItem("todos",JSON.stringify(this.todos));
  },
 
  methods: {
    isTaskShow(){
        if(this.todos.length>0)
        {return true}
        else
        {return false}
    },
    edit(e){
      console.log('edit....',this.taskName);
      let editedTaskName  = e.target.value

      if(this.isEdit){
        console.log("eeeeeeeeeeeeeeeeeeeeeeeeeee");

        this.todos.filter((el)=> { if(el.edit){el.title = this.taskName;el.title=editedTaskName}
                                  })
      }

     
      
      
    },

    taskEdit(todo,index){
      this.isEdit = true
      console.log(todo);
      todo.edit = true
      this.taskName = todo.title
      //this.taskName = todo.id++
      
      
      //console.log('taskEdit:',todo,'isEdit:',this.isEdit)
      
    },

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

      
      if(isEdit){
        this.todos.push({
          
          //id:this.todos[(this.todos.length)-1].id+1,
          id:Math.floor((Math.random()*1000000+1)),
          title:this.taskName,
          time:new Date().toLocaleString("en-US",{hour12: true}),
          done:false,
          isEdit:false
        })
      }
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
