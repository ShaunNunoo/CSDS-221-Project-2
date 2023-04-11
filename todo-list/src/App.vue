<template>
  <v-app id = "App">
    <v-main>    
      <v-container fluid>  
        <v-banner id = "bannerHeader">   
          <v-row style="padding: 5px; align-content: center; height: 50px">
              <v-col></v-col>
              <v-col style="align-content: center;">
                <subtitle-1 style="padding: 5px;"><fa icon="fa-bars" /> FRAMEWORKS</subtitle-1> 
              </v-col>
              <v-col style="padding: 5px; align-content: center;" >
                <v-row style="justify-content: end; width:100%;height: ; ">
                  <AddTask style="margin-top:12px;" @addNewTask="(newTask)=>addTask(newTask)" :taskTitles="titles"/>
                </v-row>    
              </v-col>
          </v-row>
        </v-banner>
        <v-banner id = "bannerBody">
          <v-table>
            <thead>
              <tr>
                <th style="font-weight: bold;" :rowspan="1" class="text-center">Title</th>
                <th style="font-weight: bold;" :colspan="1" class="text-center">Description</th>
                <th style="font-weight: bold;" :colspan="1" class="text-center">Deadline</th>
                <th style="font-weight: bold;" :colspan="1" class="text-center">Priority</th>
                <th style="font-weight: bold;" :colspan="1" class="text-center">Is Complete</th>
                <th style="font-weight: bold;" :colspan="1" class="text-center">Action</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(tableTask, k) in tableTasks" :key="k">
                <td style="width:;" class="text-center">{{tableTask.title}}</td>
                <td class="text-center">{{tableTask.description}}</td>
                <td class="text-center">{{tableTask.deadline}}</td>
                <td  class="text-center">{{tableTask.priority}}</td>
                <td style="text-align: center;">   
                  <input 
                  type="checkbox" 
                  style=" transform : scale(1.3); " 
                  @click="completeTask(tableTask, !tableTask.isComplete)" 
                  :checked="tableTask.isComplete?true:false">
                </td>
                <td  class="text-center">
                  <UpdateTask   
                    v-if=!tableTask.isComplete  
                    style="width: 100px;" 
                    :task="tableTask" 
                    @updateTask="(task)=>updateTask(tableTask,task)"/>
                    
                    <v-btn 
                    @click="deleteTask(tableTask)" 
                    id ="deleteButton"
                    >
                     <fa icon="fa-times-circle"/>
                      &nbsp;DELETE
                    </v-btn>
              </td> 
              </tr>

            </tbody>
          </v-table>
        </v-banner>   
      </v-container>
    </v-main>
  </v-app>
</template>

<style>

 #bannerHeader{
  background-color: #1565C0;
  color:white;
  text-align: center;
  border-top-right-radius: 7px;
  border-top-left-radius: 7px;
 }

 #bannerBody{
  border-bottom-right-radius: 7px;
  border-bottom-left-radius: 7px;
 }

 #addButton{
  background-color: #1874E2;
  color:white;
  width: 100px !important;
 }

 #deleteButton{
  background-color: #FC3C3A;
  color: white;
  width: 100px;
 }


 table {
  table-layout: fixed ;
  width: 100% ;
}
 
</style>

<script>


import AddTask from '@/components/AddTask.vue'
import UpdateTask from '@/components/UpdateTask.vue'
import toastr from 'toastr';
import 'toastr/build/toastr.min.css';

toastr.options = {
  "positionClass": "toast-bottom-right",
}

export default {
  name: 'App',
  components: {AddTask, UpdateTask},

 data(){
  return{
    tableTasks: [],
    titles: []
  }
 },

 methods:{

 addTask(newTask) {
      this.tableTasks.push(newTask);
      this.titles.push(newTask.title);
      toastr.success('Task Successfully Added!');
},

 deleteTask(task) {
      var index = this.tableTasks.indexOf(task);
      this.tableTasks.splice(index, 1);   
      this.titles.splice(index, 1);
      toastr.success('Task Successfully Deleted!');
},

updateTask(oldTask, newTask){
  var index = this.tableTasks.indexOf(oldTask);
      this.tableTasks[index] = newTask;
      toastr.success('Task Successfully Updated!');
},

 completeTask(task, isComplete){
      var index = this.tableTasks.indexOf(task);
      this.tableTasks[index].isComplete = isComplete;    
}
 }

}

</script>
