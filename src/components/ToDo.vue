<template>
  <v-alert density="comfortable" type="success" variant="tonal" v-model="deletedAlert">
      Task deleted
  </v-alert>
  <v-alert density="comfortable" type="success" variant="tonal" v-model="updatedAlert">
      Task updated
  </v-alert>
  <v-container class="ma-4">
    <v-row>
    <v-col class="col-12"></v-col>
    <v-col class="col-12"><div class="text-h4  text-center"><i class="fa fa-bars"></i> FRAMEWORKS</div></v-col>
    <v-col class="col-12 mt-3" >
      <v-row justify="center">
      <v-dialog v-model="dialog" class="w-50">
        <template v-slot:activator="{ props }">
          <v-btn color="primary" v-bind="props" class="align-right">
            <i class="fa fa-plus-circle"></i> Add
          </v-btn>
        </template>
        <v-card>
          <v-card-title>
            <span class="text-h6 ma-6"><i class="fa fa-plus-circle"></i> Add Task</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col class="w-75">
                  <v-text-field :rules="[v => !!v || 'Field is required']" id="titleInput" label="Title" variant="outlined" required></v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col class="w-75">
                  <v-text-field :rules="[v => !!v || 'Field is required']" id="descInput" label="Description" variant="outlined" required></v-text-field>
                </v-col>
              </v-row>
              <v-row>
                <v-col>
                  <v-menu
                    ref="menu"
                    v-model="menu"
                    :close-on-content-click="false"
                    :return-value.sync="date"
                    transition="scale-transition"
                    offset-y
                    min-width="auto"
                  >
                    <template v-slot:activator="{ props }">
                      <v-text-field
                        v-model="date"
                        :rules="[v => !!v || 'Field is required']"
                        requried
                        label="Due Date"
                        id="dateInput"
                        prepend-icon="mdi-calendar"
                        v-bind="props"
                        variant="outlined"
                      ></v-text-field>
                    </template>
                    <v-date-picker v-model="date" no-title scrollable>
                      <v-btn text color="primary" @click="menu = false">
                        Cancel
                      </v-btn>
                      <v-btn text color="primary" @click="$refs.menu.save(date)">
                        OK
                      </v-btn>
                    </v-date-picker>
                  </v-menu>
                </v-col>
              </v-row>
              <v-row>
                <v-container fluid>
                <!--v-col class="w-75 text-justify"-->
                  <v-radio-group v-model="inline" inline label="Priority" id="priorityInput">
                    <v-col class="align-center"><v-radio name="priority" label="Low" value ="Low"></v-radio></v-col>
                    <v-col class="align-center"><v-radio name="priority" label="Medium" value ="Medium"></v-radio></v-col>
                    <v-col class="align-center"><v-radio name="priority" label="High" value ="High"></v-radio></v-col>
                  </v-radio-group>
                </v-container>
              </v-row>
            </v-container>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="blue-darken-1" variant="text" outlined @click="dialog = false">
            Close
            </v-btn>
            <v-btn color="blue-darken-1" variant="text" @click="submitTask(), dialog = false">
                Save
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>
  </v-col>
  </v-row>
  </v-container>
  
      
  <!--/v-container>
  <v-container class="ma-4"-->
    
  <v-container>
    <v-table>
      <thead>
        <tr>
          <th class="text-center">
            Title
          </th>
          <th class="text-center">
            Description
          </th>
          <th class="text-center">
            Deadline
          </th>
          <th class="text-center">
            Priority
          </th>
          <th class="text-center">
            Is Complete
          </th>
          <th class="text-center">
            Action
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>{{task.name}}</td>
          <td>{{task.description}}</td>
          <td>{{task.deadline}}</td>
          <td>{{task.priority}}</td>
          <td><v-checkbox v-model="tasks[index].isComplete"/></td>
          <td>
            <v-container v-if="!tasks[index].isComplete">
              <v-dialog v-model=tasks[index].isUpdate class="w-50">
                <template v-slot:activator="{ props }">
                  <v-btn class="mb-0" color="blue" v-bind="props">
                    Update
                  </v-btn>
                </template>
                <v-card>
                  <v-card-title>
                    <span class="text-h6 ma-6"><i class="fa fa-pen"></i> Update Task</span>
                  </v-card-title>
                  <v-card-text>
                    <v-container>
                      <v-row>
                        <v-col class="w-75">
                          <v-text-field id="titleUpdate" label="Title" variant="outlined" v-bind:model-value="tasks[index].name" disabled></v-text-field>
                        </v-col>
                      </v-row>
                      <v-row>
                        <v-col class="w-75">
                          <v-text-field id="descUpdate" label="Description" variant="outlined" v-bind:model-value="tasks[index].description" required></v-text-field>
                        </v-col>
                      </v-row>
                      <v-row>
                        <v-col>
                          <v-menu
                            ref="menu"
                            v-model="menu"
                            :close-on-content-click="false"
                            :return-value.sync="date"
                            transition="scale-transition"
                            offset-y
                            min-width="auto"
                          >
                            <template v-slot:activator="{ props }">
                              <v-text-field
                                v-model="tasks[index].deadline"
                                label="Due Date"
                                id="dateUpdate"
                                prepend-icon="mdi-calendar"
                                v-bind="props"
                                variant="outlined"
                              ></v-text-field>
                            </template>
                            <v-date-picker v-model="date" no-title scrollable>
                              <v-btn text color="primary" @click="menu = false">
                                Cancel
                              </v-btn>
                              <v-btn text color="primary" @click="$refs.menu.save(date)">
                                OK
                              </v-btn>
                            </v-date-picker>
                          </v-menu>
                        </v-col>
                      </v-row>
                      <v-row>
                        <v-container fluid>
                          <v-radio-group v-model="inline" inline label="Priority" id="priorityUpdates">
                            <v-col class="align-center"><v-radio name="priorityUpdate" label="Low" value ="Low"></v-radio></v-col>
                            <v-col class="align-center"><v-radio name="priorityUpdate" label="Medium" value ="Medium"></v-radio></v-col>
                            <v-col class="align-center"><v-radio name="priorityUpdate" label="High" value ="High"></v-radio></v-col>
                          </v-radio-group>
                        </v-container>
                      </v-row>
                    </v-container>
                  </v-card-text>
                  <v-card-actions>
                    <v-spacer></v-spacer>
                    <v-btn color="blue-darken-1" variant="text" @click="tasks[index].isUpdate = false">
                    Close
                    </v-btn>
                    <v-btn color="blue-darken-1" variant="outlined" @click="updateTask(index), tasks[index].isUpdate = false">
                        Save
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </v-container>
            <v-btn class = "mb-2 mt-0" color="red" @click="deleteTask(index)">Delete</v-btn>
          </td>
        </tr>
      </tbody>
    </v-table>
  </v-container>
</template>

<script>
//import dialog from './dialog.vue';
export default {
  name:"ToDo",
  data() {
    return {
      dialog: false,
      updatedialog: false,
      updatedAlert: false,
      date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
      menu: false,
      deletedAlert: false,
      inline: null,
      name: '',
      description: '',
      deadline: '',
      priority: '',
      isComplete: '',
      isUpdate: false,
      tasks: [
        {
          name: 'Groceries',
          description: 'Get food from the store',
          deadline: '11/24/22',
          priority: 'High',
          isComplete: false,
          isUpdate: false
        },
        {
          name: 'Groceries',
          description: 'Get food from the store',
          deadline: '11/24/22',
          priority: 'High',
          isComplete: false,
          isUpdate: false
        }
      ]
    }
  },

  methods: {
    submitTask(){
      var priorities = document.getElementsByName('priority');
      var priorityvalue;
      for(const selection of priorities) {
        if(selection.checked) {
          priorityvalue = selection.value;
        }
      }
      this.tasks.push({
        name: document.getElementById('titleInput').value,
        description: document.getElementById('descInput').value,
        deadline: document.getElementById('dateInput').value,
        priority: priorityvalue,
        isComplete: false,
        isUpdate: false
      })
    },
    deleteTask(index) {
      this.tasks.splice(index,1);
      this.deletedAlert = true;
      setTimeout(() => {this.deletedAlert=false;}, 2000)
    },
    updateTask(index) {
      var priorities = document.getElementsByName('priorityUpdate');
      var priorityvalue;
      for(const selection of priorities) {
        if(selection.checked) {
          priorityvalue = selection.value;
        }
      }
      this.tasks[index].description = document.getElementById('descUpdate').value;
      this.tasks[index].deadline =  document.getElementById('dateUpdate').value;
      this.tasks[index].priority =  priorityvalue;
      this.tasks[index].isComplete = false;
      this.tasks[index].isUpdate = false;
      this.updatedAlert = true;
      setTimeout(() => {this.updatedAlert=false;}, 2000)
    }
  }
}
</script>
<style>
  td {
    text-align: center;
  }
  v-dialog v-text-field {
    variant: "outlined";
  }
</style>
