<template>
  <div class="container">
  <h2 class="text-center mt-5">My Vue Todo App</h2>
  <div class="d-flex">
    <input v-model="task" type="text" placeholder="請輸入代辦事項" class="form-control" @keyup.enter="addtask">
  <button @click="submitTask" class="btn btn-warning" id="button" >SUBMIT</button>
  </div>

  <!-- task table -->
  <table class="table table-bordered mt-5">
  <thead>
    <tr>
      <th scope="col">代辦事項</th>
      <th scope="col">狀態</th>
      <th scope="col" class="text-center">編輯</th>
      <th scope="col" class="text-center">刪除</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="(task,index) in tasks" :key="index">
      <td>
        <span :class="{'finished': task.status === '完成'}">
        {{task.name}}
        </span>
        </td>
      <td style="width:120px"><span @click="change(index)" class="pointer"
      :class="{'text-danger': task.status === '代辦事項',
                'text-warning': task.status === '進行中',
                'text-success': task.status === '完成' }"
      >
        {{firstCharUpper(task.status)}}
        </span>
        </td>
      <td>
        <div class="text-center" @click="editTask(index)">
        <span class="fa fa-pen"></span>
        </div>
      </td>
      <td>
         <div class="text-center" @click="deleteTask(index)">
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
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
  return{
    task:"",
    editedTask:null,
    available :["代辦事項","進行中","完成"],
    tasks:[
    ]
  }
  },
  methods:{
    submitTask(){
      if(this.task.length === 0) return;
      if(this.editedTask ===null){
         this.tasks.push({
        name:this.task,
        status:"代辦事項"
      });
      }else{
        this.tasks[this.editedTask].name = this.task.name;
        this.editedTask = null;
      }
      this.task= "";
    },
    addtask(){
      if(this.task.length === 0) return;
      if(this.editedTask ===null){
         this.tasks.push({
        name:this.task,
        status:"代辦事項"
      });
      }else{
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      }
      this.task= "";
    },
     deleteTask(index){
       if(this.tasks[index].status === "完成"){
          this.tasks.splice(index,1)
       }
      },
      editTask(index){
        this.task = this.tasks[index].name;
        this.editedTask = index;
      },
      change(index){
        let newIndex = this.available.indexOf(this.tasks[index].status);
        if(++newIndex > 2) newIndex = 0;
        this.tasks[index].status = this.available[newIndex];
      },
      firstCharUpper(str){
        return str.charAt(0).toUpperCase() + str.slice(1);
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer{
  cursor: pointer;
}
.finished{
  text-decoration: line-through;
}
#button{
  margin-left: 10px;
}
</style>
