<template>
  <div class="page-container">
    <div class="background-image"></div>
    <div class="content-container">
      <br/>
      <div class="row">
        <div class="col-md-8">
          <h1>Welcome to My Task Manager</h1>
        </div>
        <div class="col-md-4 text-right">
          <button class="btn btn-success"  style="margin-top: 10px; "   @click="openPopup">Add New Task</button>
        </div>
      </div>
      <br/>
     
      <div v-if="loading">
        <p>Loading...</p>
      </div>
      <div v-else>
        <div v-if="tasks && tasks.length > 0">
          <div class="row">
            <div v-for="task in sortedTasks" :key="task._id" class="col-md-4 mb-2 ">
              <div class="card">
                <div class="card-body">
                  <h5 class="card-title">{{ task.taskName }}</h5>
                  <p class="card-text">Description: {{ task.description }}</p>
                  <p class="card-text">Estimated Time: {{ task.estimatedTime }} hours</p>
                  <p class="card-text">Priority: {{ task.priority }}</p>
                  <p class="card-text">Completion: {{ task.completion }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div v-else>
          <p>No tasks available.</p>
        </div>
      </div>
    </div>
   
     
 
     <!-- Display the popup directly on the current page -->
     <div v-if="isPopupOpen" class="modal">
           <div class="modal-content">
             <form @submit.prevent="addTask">
               <h2 class="modal-title">Add New Task</h2>
               <div>
           <label for="editTaskName">Task Name:</label>
          <input v-model="taskName" type="text" id="editTaskName" class="form-control"  />
          </div>
          <div>
           <label for="editDescription">Description:</label>
          <input v-model="description" type="text" id="editDescription" class="form-control"  />
          </div>
          <div>
           <label for="editEstimatedTime">Estimated Time:</label>
          <input v-model="estimatedTime" type="number" id="editEstimatedTime" class="form-control"  />
          </div>
          <div>
           <label for="editPriority">Priority:</label>
           <select v-model="priority" class="form-control">
             <option value="High">High</option>
             <option value="Medium">Medium</option>
             <option value="Low">Low</option>
           </select>
         </div>
         <div>
           <label for="editCompletion">Completion:</label>
           <select v-model="completion" class="form-control">
             <option value="yes">Yes</option>
             <option value="no">No</option>
           </select>
         </div>    
        </form>
        <br />
         <div class="d-flex justify-content-between">
               <button type="submit" class="btn btn-success" @click="addTask">Add New Task</button>
               <button type="button" class="btn btn-secondary" @click="closePopup">Close</button>
             </div>
           </div>
         </div>
     
  </div>
</template>

<script>
import axios from 'axios';
import AddTaskModal from '@/components/common/AddTaskModal.vue';
export default {
  components: {
    AddTaskModal,
  },
  props: ['task'],
  data() {
    return {
      taskName: '',
    description: '',
    estimatedTime: 0,
    priority: '',
    completion: '',
    loading: false,
    showAddTaskModal: true,
    isPopupOpen: false,
    error: '',
    tasks: [],
    };
  },
  created() {

  this.fetchTasks();
},
computed: {
sortedTasks() {
  return this.tasks.slice().sort((a, b) => {
    const priorityOrder = { High: 1, Medium: 2, Low: 3 };
    return priorityOrder[a.priority] - priorityOrder[b.priority];
  });
},
},
  methods: {
    openPopup() {
       this.isPopupOpen = true;
     },
     closePopup() {
       this.isPopupOpen = false;
     },
     async addTask() {
       this.loading = true;
 
       try {
         const response = await axios.post('http://localhost:8091/Task/addPost', {
           taskName: this.taskName,
           description: this.description,
           estimatedTime: this.estimatedTime,
           priority: this.priority,
           completion: this.completion,
           cookie: '', // Set your cookie value here
         }, {
           withCredentials: true, // Include credentials in the request
         });
 
         if (response.data.message) {
           // Task added successfully
           console.log(response.data.message);
           // Add any additional logic or navigation here
         } else {
           // Handle unsuccessful task addition
           this.error = 'Task addition failed. Please try again.';
         }
       } catch (error) {
         // Handle errors here
         console.error('Task addition failed', error);
         this.error = 'Task addition failed. Please try again.';
       } finally {
         this.loading = false;
       }
     },
 
  async fetchTasks() {
    try {
      const response = await axios.get('http://localhost:8091/Task/getAllTasks', {
        withCredentials: true,
      });
      console.log(response.data);
      this.tasks = response.data;
    } catch (error) {
      console.error('Error fetching tasks:', error);
    }
  },

  },
};
</script>
<style scoped>

 .content-container {
   z-index: 1;
   text-align: center;
 }
 
.background-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url('https://i.postimg.cc/W1jRkhb7/th-2.jpg'); /* Replace with your image URL */
  background-size: cover;
  filter: blur(8px);
  z-index: -1;
}
 .modal {
   position: fixed;
   top: 0;
   left: 0;
   width: 100%;
   height: 100%;
   background: rgba(0, 0, 0, 0.5);
   display: flex;
   justify-content: center;
   align-items: center;
 }
 
 .modal-content {
   background: white;
   padding: 20px;
   border-radius: 5px;
   margin-left: 350px;
   margin-right:350px;
 }

</style>