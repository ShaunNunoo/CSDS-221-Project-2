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
            id = "addButton"
          >
            <fa icon="fa-plus-circle" />&nbsp;ADD
              </v-btn>
        </template>
  
        <v-card>
          <v-card-text>
            <fa icon="fa-plus-circle" />&nbsp;<subtitle-1>Add Task</subtitle-1>
          </v-card-text>
          <v-card-actions>
            <v-container style="padding: 0px;">
              <v-container style="padding: 30px;">
                <v-row>
                    <v-text-field 
                    variant="outlined" 
                    placeholder="Title"
                    label="Title"  
                    ref="title"
                    v-model="title"
                    :error-messages="errorMessages"
                    required
                    :rules="[
                      () => !!title || 'Title is Required!',
                      () => !!title && this.taskTitles.indexOf(title.toString()) == -1|| 'Title Alread Exists!'
                    ]"
                    >
                    </v-text-field>
                </v-row>

                <v-row> 
                    <v-text-field 
                    variant="outlined"  
                    placeholder="Description"
                    label="Description"  
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
                    name="priority"  
                    checked 
                    @click="changePriority('low')"/>
                    <label>&nbsp; Low </label>
                    <input 
                    style="margin-left: 30px;" 
                    type="radio" 
                    class="radio"
                    name="priority"  
                    @click="changePriority('med')"/>
                    <label>&nbsp; Med</label>
                    <input
                    style="margin-left: 30px;" 
                    type="radio" 
                    class="radio"  
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
                    <subtitle-1><fa icon="fa-plus-circle" />&nbsp;ADD</subtitle-1>
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
        tableTasks: tableTasks,
        title: null,
        description: null,
        deadline: null,
        formHasErrors: false,
        priority: "low",
        date: null
      }

    },

    computed: {
      form () {
        return {
          title: this.title,
          description: this.description,
          deadline: this.deadline
        }
      },
    },

    props: [
      'taskTitles'
    ],

    methods: {
      changePriority(priority){
        this.priority = priority;
      },

      resetForm () {
        this.errorMessages = []
        this.formHasErrors = false;

        Object.keys(this.form).forEach(f => {
          this.$refs[f].reset()
        })
      },

      submit () {
        this.formHasErrors = false
        Object.keys(this.form).forEach(f => {
          if (!this.form[f]) 
          this.formHasErrors = true
          this.$refs[f].validate(true)
        })
        
        
        if(!this.formHasErrors && this.taskTitles.indexOf(this.title.toString()) == -1){
          var date = this.deadline.toString();
          var newFormat = date.substring(5,7) + "/" +date.substring(8,10) + "/" +date.substring(2,4);

          var newTask ={
            title: this.title,
            description: this.description,
            deadline: newFormat,
            priority: this.priority,
            isComplete: false
          }


          this.$emit('addNewTask', newTask);
          this.dialog = false;
          this.resetForm();
        }
      },

        closePopUp(){ 
          this.dialog = false;
          this.resetForm();
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

