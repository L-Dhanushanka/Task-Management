<template>
  <div class="page-container">
    <div class="background-image"></div>
    <div class="content-container">
      <!-- <button @click="fetchTasks">Fetch Tasks</button> -->
      <div v-if="loading">
        <p>Loading...</p>
      </div>
      <div v-else>
        <div v-if="users && users.length > 0">
          <br/>
          <h2>Users Added for the system</h2>
          <div class="search-container">
            <!-- Bootstrap input group for search -->
            <div class="input-group mb-3 ml-1"  style="max-width: 300px; margin-left: 100px;">
              <input
                v-model="searchQuery"
                type="text"
                class="form-control"
                placeholder="Search "
              />
              <div class="input-group-append">
                <button @click="searchUsers" class="btn btn-outline-secondary" type="button">
                  Search
                </button>
              </div>
            </div>
          </div>
          <div class="card-container">
            <div v-for="users in users" :key="users._id" class="card">
              <h3>{{ users.customername  }}</h3>
              <p>Company: {{ users.company  }}</p>
              <p>Contact: {{ users.contact  }}</p>
              <p>Country: {{ users.country  }}</p>
              <p>Email: {{ users.email  }}</p>  
            </div>
          </div>
        </div>
        <div v-else>
          <p>No tasks available.</p>
        </div>
      </div>
      
    </div>
  </div>
</template>

<script>

import axios from 'axios';

export default {
  data() {
    return {
      users: [],
      searchQuery: '',
      loading: false,
      taskName: "",
    description: "",
    estimatedTime: 0,
    priority: "High" ,
    completion: "yes"
    };
  },
  async fetch() {
    await this.fetchUsers();
  },
  methods: {
    async fetchUsers() {
      try {
        const response = await axios.get(
          'http://localhost:8091/User/getAllUsers',
          { withCredentials: true }
        );
        this.users = response.data;
      } catch (error) {
        console.error('Error fetching users:', error);
      }
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
    width: 340px; /* Set your desired width */
    height: 230px; /* Set your desired height */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    text-align: left;
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

</style>