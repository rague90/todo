<template>
    <div class="container" style="max-width:600px;text-align: center">
        <h2 style="mt-20"> todo App</h2>
        <div class="d-flex mt-5">
    
        <input 
        type="text"
        v-model="task"
        placeholder="Enter task"
        class="w-100 form-control"
        />
     <button type="button" class="btn btn-info rounded-2 m-1" @click="submitTask">Submit</button>
        </div>

       <table class="table table-bordered mt-3" >
  <thead>
 
    <tr>
      <th scope="col">Task</th>
      <th scope="col" style="width: 120px">Status</th>
      <th scope="col" style="width: 120px">Edit</th>
      <th scope="col" style="width: 120px">Delete</th>
    </tr>
      </thead>
<tbody>
       <tr v-for="(task,index) in tasks" :key="index">
        <td>
            <span :class="{ 'line-through': task.status === 'finished' }">
              {{ task.name }}
            </span>
        </td>
        <td>
              <span
              class="pointer select"
              @click="changeStatus(index)"
              :class="{
                'text-danger': task.status === 'to-do',
                'text-success': task.status === 'finished',
                'text-warning': task.status === 'in-progress',
              }"
            >
            {{task.status}}
            </span>
            
        </td>
        <td class="text-center">
            <div @click="editask(index)"><span class="fa fa-pen"></span></div></td>
        <td class="text-center">
            <div @click="deleteTask(index)"><span class="fa fa-trash"></span></div></td>
    </tr>

</tbody>

</table>
               
    </div>
</template>

<script>
export default {
   name:   'TodoApp',
   props  :{
        msg:String,
   },
   data() {
    return {
        task:"",        
       editTask:null,
        statuses:["to-do","in-progress","finished"],
        tasks:[
            {
               name:"webcodecamp.",
               status:"to-do",
            },
            {
               name:"subscribe now.",
               status:"in-progress",
            },
            {
                name:"Create Youtube video",
                status:"finished",
            },
        ],
    }
   },
   methods: {
    
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },
  /**
     * Deletes task by index
     */
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },

    /**
     * Edit task
     */
    editask(index) {
      this.task = this.tasks[index].name;
      this.editTask= index;
    },
   submitTask() {
      if (this.task.length === 0){
       return alert("Enter Task !!!");
      }

  
      if (this.editTask != null) {
        this.tasks[this.editTask].name = this.task;
        this.editTask = null;
      } else {
        /* We need to add new task */
        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }

      this.task = "";
    },

  },  
}
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
.noselect {
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome, Edge, Opera and Firefox */
}
.line-through {
  text-decoration: line-through;
}
</style>
