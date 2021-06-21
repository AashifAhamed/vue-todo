<template>
  <div class="container">
    <h2 class="text-center mt-5">My Vue ToDo App</h2>
  

    <div class="d-flex">
       <input v-model="task" type="text" placeholder="Enter Task" class="form-control">
       <button @click="submitTask" class="btn btn-success rounded-0">Submit</button>
    </div>

    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Tasks</th>
          <th scope="col">Status</th>
          <th scope="col">#</th>
          <th scope="col">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task,index) in tasks" :key="index">
          <td>
            <span :class="{'finished':task.status === 'completed'}">{{task.name}}</span>
          </td>
          <td style="width:120px">
            <span @click="changeStatus(index)" class="pointer"
            :class="{'text-danger':task.status === 'to-do',
            'text-warning':task.status === 'in-progress'}"
            >
              {{firstLetterToUpper(task.status)}}
            </span>
          </td>
          <td>
            <div class="text-center btn-warning" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center btn-danger" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'TodoApp',
  props: {
    msg: String
  },

  data(){
    return{
      task: '',
      editedTask: null,
      avilalbleStatuses: ['to-do','in-progress','completed'],
      tasks:[ 
        {
          name: 'Buy banana.',
          status: 'to-do'
        },
        {
          name: 'Sell banana.',
          status: 'to-do' 
        }
      ]
    }
  },

  methods:{
    submitTask(){
      if(this.task.length === 0) return;

      if(this.editedTask === null){
        this.tasks.push({
          name:this.task,
          status:'to-do'
        });
      }else{
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }

      this.task = '';
    },

    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index
    },
    deleteTask(index){
      this.tasks.splice(index,1);
    },

    changeStatus(index){ 
      let newIndex = this.avilalbleStatuses.indexOf(this.tasks[index].status);
      
      if(++newIndex > 2) newIndex = 0;
      
      this.tasks[index].status = this.avilalbleStatuses[newIndex];
    },

    firstLetterToUpper(str){
      return str.charAt(0).toUpperCase() + str.slice(1);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .pointer{
   cursor: pointer; 
  }
  .finished{
    text-decoration:line-through;
  }
</style>
