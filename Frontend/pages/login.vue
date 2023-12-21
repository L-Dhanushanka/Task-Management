<template>
    <div class="content-container">  
    <div class="background-image"></div>
    <form @submit.prevent="login" class="login-form">
      <h1>Login</h1>
      <label for="customername" >Customer Name:</label>
      <input v-model="customername" class="form-control" type="text" id="customername" required />

      <label for="password" >Password:</label>
      <input v-model="password" class="form-control" type="password" id="password" required />

      <div v-if="loginError" class="error-message">{{ loginError }}</div>
      <br/>
      <button type="submit" :disabled="loading">
        <span v-if="loading">Logging in...</span>
        <span v-else>Login</span>
      </button><br/><br/>

     

    </form>
  

  </div>
</template>

<script>
import axios from 'axios';

export default {
  components: {
  },
  data() {
    return {
      customername: '',
      password: '',
      loading: false,
      loginError: '',
      loading: false,
      registrationError: '',
      

    };
  },
  methods: {
    async login() {
      this.loading = true;
      try {
        const response = await axios.post('http://localhost:8091/User/login', {
          customername: this.customername,
          password: this.password,
        },
        {
          withCredentials: true,
        });
       
     
        if (response.data.status) {
          // Navigate to the home page
          // console.log(response.data.data.user);
          console.log(response.data.data.customername);
          console.log(response.data.data._id);
          localStorage.setItem('customername', response.data.data.customername);
          localStorage.setItem('customerId', response.data.data._id);
          this.$router.push('/task');
        } else {
          // Handle unsuccessful login
          this.loginError = 'Invalid credentials. Please try again.';
        }

        // Redirect to another page or perform other actions based on the response
      } catch (error) {
        // Handle errors here
        console.error('Login failed', error);
        this.loginError = 'Invalid credentials. Please try again.';
      } finally {
        this.loading = false;
      }
    },
  
  
  },
};
</script>

<style scoped>
/* Add your styles here */

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

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
.login-form {
  max-width: 400px;
  margin: 0 auto;
  margin-top: 150px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  color: #333;
}

label {
  display: block;
  margin: 10px 0 5px;
  color: #555;
}

input {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  box-sizing: border-box;
}

button {
  background-color: #4caf50;
  color: white;
  padding: 10px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  width: 100%;
}

button:hover {
  background-color: #45a049;
}

.error-message {
  color: red;
  margin-top: 10px;
  text-align: center;
}

button:disabled {
  cursor: not-allowed;
  background-color: #b8b8b8;
}
</style>
