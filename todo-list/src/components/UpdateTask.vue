<template>
    <div id  = "AddTask">
      <v-dialog
        v-model="dialog"
        width="auto"
      >
        <template v-slot:activator="{ props }">
          <v-btn
            v-bind="props"
            style="margin-right: 10px;"
            id="updateButton"
          >
            <fa icon="fa-pen-to-square"/>&nbsp;UPDATE
            </v-btn>
        </template>
  
        <v-card>
          <v-card-text>
            <fa icon="fa-pen-to-square"/>&nbsp;<subtitle-1>Edit Task</subtitle-1>
          </v-card-text>
          <v-card-actions>
            <v-container style="padding: 0px;">
              <v-container style="padding: 30px;">
                <v-row> 
                    <v-text-field 
                    variant="outlined"
                    label="Description"  
                    placeholder="Description"
                    ref="description"
                    v-model="description"
                    :error-messages="errorMessages"
                    required
                    :rules="[() => !!description || 'Description is Required!']"
                    >

                    </v-text-field>
                </v-row>

                <v-row style="height: 80px;">
                    <v-text-field 
                    type="date"
                    variant="outlined"  
                    label="Deadline"
                    ref="deadline"
                    v-model="deadline"
                    :error-messages="errorMessages"
                    :rules="[() => !!deadline || 'Deadline is Required!']"
                    >

                    </v-text-field>
                </v-row>
                <v-row>
                    <label style="margin-bottom: 10px;">Priority</label>
                </v-row>
                <v-row style="justify-content:start; align-content: center; margin-bottom: 10px;">
                    <input 
                    type="radio"
                     class="radio"   
                     :checked="task.priority == 'low'?true:false"
                     name="priority"  
                    @click="changePriority('low')"/>
                    <label>&nbsp; Low </label>
                    <input 
                    style="margin-left: 30px;" 
                    type="radio" 
                    class="radio" 
                    :checked="task.priority == 'med'?true:false"
                    name="priority"  
                    @click="changePriority('med')"/>
                    <label>&nbsp; Med</label>
                    <input 
                    style="margin-left: 30px;" 
                    type="radio" 
                    class="radio" 
                    :checked="task.priority == 'high'?true:false"
                    name="priority"  
                    @click="changePriority('high')"/>
                    <label>&nbsp; High</label>
                </v-row>

              </v-container>
            <v-container>
                <v-row style="justify-content: end;">
                  <v-btn 
                    style="margin-right: 10px;" 
                    @click="submit" 
                    id = "addButton">
                    <subtitle-1><fa icon="fa-pen-to-square" />&nbsp;EDIT</subtitle-1>
                    </v-btn>
                    <v-btn @click="closePopUp" id ="cancleButton">
                      <subtitle-1>
                        <fa icon="fa-ban"/>&nbsp;CANCLE
                      </subtitle-1>
                    </v-btn>
                </v-row>
              </v-container>
            </v-container>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
  </template>

<script> 
import {tableTasks} from '@/App.vue'


    export default {
    name: 'AddTask',
    
    data () {
      return {
        dialog: false,
        lowRadio: this.lowRadio,
        tableTasks: tableTasks,
        description: this.task.description,
        deadline: new Date(this.task.deadline.toString()).toJSON().substring(0,10),
        formHasErrors: false,
        updatedTask: this.task,
        priority: this.task.priority,
      }

    },

    computed: {
      form () {
        return {
          description: this.description,
          deadline: this.deadline
        }
      },
    },

    props: [
      'task'
    ],

    methods: {
      changePriority(priority){
        this.priority = priority;
      },

      resetForm () {
        this.errorMessages = []
        this.formHasErrors = false;

        Object.keys(this.form).forEach(f => {
          this.$refs[f].reset();
        })
      },

      undoFormat(date){
        return new Date(date);
      },

      submit () {
        this.formHasErrors = false
        
        Object.keys(this.form).forEach(f => {
          if (!this.form[f]) 
          this.formHasErrors = true
          this.$refs[f].validate(true)
        })
        
        
        if(!this.formHasErrors){
        var date = this.deadline.toString();
        var newFormat = date.substring(5,7) + "/" +date.substring(8,10) + "/" +date.substring(2,4);
        this.updatedTask.deadline = newFormat;
        this.updatedTask.priority = this.priority;
        this.updatedTask.description = this.description;

        this.$emit('updateTask', this.updatedTask);     
          this.dialog = false;
        }
      },

        closePopUp(){
          this.dialog = false;
        }, 
    }


  }

</script>

<style>
.radio{
    transform : scale(1.3); 
}
#addButton{
  background-color: #1874E2;
  color:white;
  width: 100px;
  
 }

 #updateButton{
  background-color: #1874E2;
  color:white;
  width: 100px;
 }

 .v-card-actions{
    width: 300px;
 }

 .v-card-text{
    background-color: #1565C0;
    color:white; 
 }

 #cancleButton{
  background-color: #FC3C3A;
  color: white;
  width: 100px;
 }

 .v-text-field{
    margin-bottom: 20px;
 }

</style>

