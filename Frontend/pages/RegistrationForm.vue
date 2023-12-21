<template>
    <div class="content-container ">
        
          <div class="background-image"></div>
          <form @submit.prevent="register" class="login-form">
            <h1 class="card-title">Register</h1>
            <div class="row mb-2">
              <label for="customername" class="col-sm-3 col-form-label">Customer Name:</label>
              <div class="col-sm-9">
              <input v-model="customername" type="text" class="form-control" id="customername" required />
            </div></div>
  
            <div class="row mb-2">
              <label for="company" class="col-sm-3 col-form-label">Company:</label>
              <div class="col-sm-9">
              <input v-model="company" type="text" class="form-control" id="company" required />
            </div>
        </div>
  
            <div class="row mb-2">
              <label for="contact" class="col-sm-3 col-form-label">Contact:</label>
              <div class="col-sm-9">
              <input v-model="contact" type="text" class="form-control" id="contact" required />
            </div>
        </div>
  
            <div class="row mb-2">
              <label for="email" class="col-sm-3 col-form-label">Email:</label>
              <div class="col-sm-9">
              <input v-model="email" type="email" class="form-control" id="email" required />
            </div>
        </div>
  



            <div class="row mb-2">
            <label for="country" class="col-sm-3 col-form-label">Country:</label>
            <div class="col-sm-9">
              <input v-model="country" type="text" class="form-control" id="country" required />
            </div>
            </div>
  



            <div class="row mb-2">
              <label for="password" class="col-sm-3 col-form-label">Password:</label>
              <div class="col-sm-9">
              <input v-model="password" type="password" class="form-control" id="password" required />
            </div>
        </div>
  
            <div class="row mb-2">
              <label for="address2" class="col-sm-3 col-form-label">Address 2:</label>
              <div class="col-sm-9">
              <input v-model="address2" type="text" class="form-control" id="address2" />
            </div>
        </div>
  
            <div class="row mb-2">
              <label for="address3" class="col-sm-3 col-form-label">Address 3:</label>
              <div class="col-sm-9">
              <input v-model="address3" type="text" class="form-control" id="address3" />
            </div>
        </div>
  
            <div v-if="registrationError" class="error-message">{{ registrationError }}</div>
            <br/><br/>
  
            <button type="submit" :disabled="loading" class="btn btn-primary">
              <span v-if="loading">Registering...</span>
              <span v-else>Register</span>
            </button>
          </form>
        </div>
 
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        customername: '',
        company: '',
        contact: '',
        email: '',
        country: '',
        address2: '',
        address3: '',
        password: '',
        loading: false,
        registrationError: '',
      };
    },
    methods: {
      async register() {
        this.loading = true;
        try {
          const response = await axios.post('http://localhost:8091/User/addUser', {
            customername: this.customername,
            company: this.company,
            contact: this.contact,
            email: this.email,
            country: this.country,
            password: this.password,
            address2: this.address2,
            address3: this.address3,
          });
  
          if (response.data.message) {
            // Registration successful
            console.log(response.data.message);
            this.$router.push('/');
          } else {
            // Handle unsuccessful registration
            this.registrationError = 'Registration failed. Please try again.';
          }
        } catch (error) {
          // Handle errors here
          console.error('Registration failed', error);
          this.registrationError = 'Registration failed. Please try again.';
        } finally {
          this.loading = false;
        }
      },
    },
  };
  </script>
  
  <style scoped>
.login-form {
  max-width: 800px;
  margin: 0 auto;
  margin-top: 15px;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.629);
}

  
  .card {
    max-width: 650px;
    width: 100%;
    height: 90%;

    border: none;
    border-radius: 15px;
  }
  
  .card-title {
    text-align: center;
    margin-bottom: 1rem;
  }
  
  .card-body {
    padding: 1rem;
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
  
  .error-message {
    color: red;
    font-size: small;
  }
  </style>
  