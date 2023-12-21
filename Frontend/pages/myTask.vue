<template>
  <div class="page-container">
    <div class="background-image"></div>
    <h1>Welcome to My Task Manager</h1>
    <div class="content-container">
     
      <div v-if="loading">
        <p>Loading...</p>
      </div>
      <div v-else>
        <div v-if="tasks && tasks.length > 0">
         

          <table class="table">
            <thead>
              <tr>
                <th>Task Name</th>
                <th>Description</th>
                <th>Estimated Time</th>
                <th>Priority</th>
                <th>Completion</th>
                <th>Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="task in sortedTasks" :key="task._id">
                  <td>{{ task.taskName }}</td>
                  <td>{{ task.description }}</td>
                  <td>{{ task.estimatedTime }} hours</td>
                  
                  <td>
                  <select v-model="task.priority" @change="updatePriority(task._id, task.priority)" class="form-control">
                      <option value="High">High</option>
                      <option value="Medium">Medium</option>
                      <option value="Low">Low</option>
                  </select>
                  </td>
                  
                    <td>{{ task.completion }}</td>
                    <!-- <td> 
                  <select v-model="task.completion" @change="updateCompletion(task._id, task.completion)" class="form-control">
                      <option value="false">Yes</option>
                      <option value="true">No</option>
                  </select>
                  </td> -->

                  <td>
                  <button @click="deleteTask(task._id)" class="btn btn-danger btn-sm">Delete Task</button>
                  <button @click="updateTask(task._id)" class="btn btn-primary  btn-sm">Update Task</button>
                  </td>
              </tr>
            </tbody>
          </table>
        </div>
        <div v-else>
          <p>No tasks available.</p>
        </div>
      </div>
    </div>

<!-- Update Task Modal -->
<!-- Update Task Modal -->
<div class="card-modal" v-if="showUpdateTaskModal">
  <div class="card-content">
    <h5 class="modal-title">Update Task</h5>
  </div>
  <div class="modal-body">
    <div>
      <label for="editTaskName">Task Name:</label>
      <input v-model="taskName" type="text" id="editTaskName" class="form-control" />
    </div>
    <div>
      <label for="editDescription">Description:</label>
      <input v-model="description" type="text" id="editDescription" class="form-control" />
    </div>
    <div>
      <label for="editEstimatedTime">Estimated Time:</label>
      <input v-model="estimatedTime" type="number" id="editEstimatedTime" class="form-control" />
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

  </div>
  <div>
    <br />
    <button type="button" class="btn btn-secondary" @click="closeCard">Close</button>
    <button type="button" class="btn btn-success" @click="submitUpdate">Update Task</button>
  </div>
</div>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  props: ['task'],
  data() {
    return {
      tasks: [],
      loading: false,
      searchQuery: "",
      showAddTaskModal: false,
      showUpdateTaskModal: false, // Initialize to false initially
      taskName: "",
      description: "",
      estimatedTime: 0,
      priority: "High",
      completion: "yes"
    };
  },
  created() {
  // Load tasks when the component is created (page is refreshed)
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
    openCard() {
    this.showUpdateTaskModal = true;
  },

  // Method to close the card
  closeCard() {
    this.showUpdateTaskModal = false;
  },
      mounted() {
  this.submitUpdate();
},

updateTask(taskId) {
    const taskToUpdate = this.tasks.find(task => task._id === taskId);
    if (taskToUpdate) {
      this.editTaskId = taskId;
      this.taskName = taskToUpdate.taskName;
      this.description = taskToUpdate.description;
      this.estimatedTime = taskToUpdate.estimatedTime;
      this.priority = taskToUpdate.priority;
      this.completion = taskToUpdate.completion;
      this.showUpdateTaskModal = true; // Set the flag to show the modal
    }
  },

  closeUpdateTaskModal() {
    this.showUpdateTaskModal = false;
    // Clear any data if needed
    this.editTaskId = null;
    this.taskName = '';
    this.description = '';
    this.estimatedTime = null;
    this.priority = 'High';
    this.completion= 'yes';
  },


async submitUpdate() {
try {
  this.loading = true;

  const updateData = {
    taskName: this.taskName,
    description: this.description,
    estimatedTime: this.estimatedTime,
    priority: this.priority,
    completion: this.completion,
  };

  const response = await axios.put(`http://localhost:8091/Task/update/${this.editTaskId}`, updateData, {
    withCredentials: true,
  });

  console.log(response.data); // Log the response from the server

  this.fetchTasks(); // Refresh the task list after update
  this.closeUpdateTaskModal();
} catch (error) {
  console.error('Error updating task:', error);
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
    async deleteTask(taskId) {
      try {
        await axios.delete(`http://localhost:8091/Task/deleteTask/${taskId}`,{
        withCredentials: true,
      });
        this.fetchTasks(); // Refresh the task list after deletion
      } catch (error) {
        console.error('Error deleting task:', error);
      }
    },
    beforeRouteLeave(to, from, next) {
  // Close the modal when navigating away from the page
  this.closeUpdateTaskModal();
  next();
},
  },
};
</script>
<style scoped>
.page-container {
  position: relative;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  color: rgb(0, 0, 0);
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

.content-container {
  z-index: 1;
  text-align: center;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.card {
  background-color: rgba(112, 109, 109, 0.177);
  border-radius: 8px;
  padding: 16px;
  margin: 16px;
  max-width: 300px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align
  : left;
}


.modal {
display: none;
position: fixed;
z-index: 1;
left: 0;
top: 0;
width: 100%;
height: 100%;
overflow: auto;
background-color: rgb(0, 0, 0);
background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
background-color: #fefefe;
margin: 10% auto;
padding: 20px;
border: 1px solid #888;
width: 80%;
}

.close {
color: #aaa;
float: right;
font-size: 28px;
font-weight: bold;
}

.close:hover,
.close:focus {
color: black;
text-decoration: none;
cursor: pointer;
}

.modal-buttons {
text-align: center;
margin-top: 20px;
}
.modal-content {
   background: white;
   padding: 20px;
   border-radius: 5px;
   margin-left: 350px;
   margin-right:350px;
 }

.card-modal {
position: fixed;
top: 50%;
left: 50%;
width: 500px;
transform: translate(-50%, -50%);
background-color: white;
padding: 20px;
border: 1px solid #ccc;
box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
z-index: 999;
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