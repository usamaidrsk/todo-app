<template>
  <div id="app">
    <h4 class="bg-primary text-white text-center p-2">
      {{user}}'s To Do List
    </h4>
    <div class="container-fluid p-4">
    <div class="row" v-if="filteredtasks.length == 0">
      <div class="col text-center">
        <b>Nothing to do. {{user}}!</b>
      </div>
    </div>
    <template v-else>
    <div class="row subtitles" >
        <div class="col font-weight-bold">Task</div>
        <div class="col-2 font-weight-bold addbtn">Done</div>
    </div>
    <div class="row" v-for="t in filteredtasks" :key="t.action">
        <div class="col tototitle">{{t.action}}</div>
        <div class="col-2 checkers">
          <input type="checkbox" v-model="t.done" class="form-check-input" />
        </div>
    </div>
    </template>
    <form class="row py-2" @submit.prevent="addNewTodo">
      <div class="col">
        <input v-model="newItemText" class="form-control addtext" />
      </div>
      <div class="col-2 addbtn">
        <button class="btn btn-primary addbtn" >Add</button>
      </div>
    </form>
    <div class="row bg-secondary py-2 mt-2 text-white footer">
      <div class="col text-center">
        <input type="checkbox" v-model="hidecompleted" class="form-check-input" />
        <label class="form-check-label font-weight-bold">Hide completed tasks</label>
      </div>
      <div class="col text-center">
        <button class="btn btn-sm btn-warning" @click="deleteCompleted">
          Delete Completed
        </button>
      </div>
    </div>
    </div>
  </div>
</template>
<script>
  export default {
    name: 'app',
    data(){
      return {
        user: "User",
        tasks: [],
        hidecompleted: true,
        newItemText: ""
      }
    },
    computed: {
        filteredtasks () {
          return this.hidecompleted ? this.tasks.filter(t => !t.done) : this.tasks;
      }
    },
    methods: {
      addNewTodo () {
        if (this.newItemText){
        this.tasks.push(
          {
            action: this.newItemText,
            done: false
          }
        );
        }
        this.storeData();
        this.newItemText = "";
      },
      storeData () {
        localStorage.setItem("todos",JSON.stringify(this.tasks));
      },
      deleteCompleted () {
        this.tasks = this.tasks.filter(t => !t.done);
        this.storeData();
      }
    },
    created() {
      let data = localStorage.getItem("todos");
      if(data != null){
        this.tasks = JSON.parse(data);
      }
    },
    }
</script>
<style >
  :root{
    /*box-sizing: border-box; */
    font-size: calc(0.6em + .7vw);
  }
  #app{
    max-width: 800px;
    margin: 5em auto auto;
    border: 1em solid grey;
  }
  .row{
    margin: .8rem;
  }
  .subtitles{
    font-size: 1.3rem;
  }
  .tototitle{
    margin-left: .8rem;
  }
  .checkers{
    margin-left: 3.5rem;
  }
  .addtext{
    min-width: 80%;
  }
  .addbtn{
    margin-right: 1.5em;
  }
  .footer{
    padding: .5em;
  }
</style>