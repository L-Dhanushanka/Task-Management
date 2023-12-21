<template>
  <div class="profile-container">
    <div class="background-image"></div>
    <div class="card">
      <div class="card-header">
        <i class="fas fa-user user-icon"></i>
        <h1>User Profile</h1>
      </div>
      <div class="card-body">
        <!-- User profile details -->
        <div v-if="user">
          <p><strong>Customer Name:</strong> {{ user.customername }}</p>
          <p><strong>Company:</strong> {{ user.company }}</p>
          <p><strong>Contact:</strong> {{ user.contact }}</p>
          <p><strong>Email:</strong> {{ user.email }}</p>
          <p><strong>Country:</strong> {{ user.country }}</p>
          <p><strong>Password:</strong> {{ user.password }}</p>
          <p><strong>Address 2:</strong> {{ user.address2 }}</p>
          <p><strong>Address 3:</strong> {{ user.address3 }}</p>
        </div>
        <div v-else>
          <p>{{ error }}</p>
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
        user: null,
        error: null,
      };
    },
    mounted() {
      this.fetchUserProfile();
    },
    methods: {
      async fetchUserProfile() {
        try {
          const customerName = localStorage.getItem('customername');
          const customerId = localStorage.getItem('customerId');
          const userId = 1; // Change this to the actual user ID or get it dynamically
          const response = await axios.get(`http://localhost:8091/User/get/${customerId}`, {
            withCredentials: true,
          });
          this.user = response.data;
        } catch (error) {
          console.error('Error fetching user profile', error);
          this.error = 'Error fetching user profile. Please try again later.';
        }
      },
    },
  };
  </script>
  
  <style scoped>
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

.profile-container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.card {
  margin-bottom: 180px;
  width: 400px;
  height: 500px;
  background-color: #fff;
  border-radius: 10px;
  overflow: hidden;
}

.card-header {
  text-align: center;
  padding: 20px;
  background-color: #3498db; /* Change the background color as needed */
  color: #fff;
}

.user-icon {
  font-size: 2em; /* Adjust the size as needed */
  margin-right: 10px;
}
</style>
  