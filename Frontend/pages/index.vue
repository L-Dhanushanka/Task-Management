<template>
  <div class="container-fluid vh-100 d-flex justify-content-center align-items-center">
    <div class="card">
      <div class="background-image"></div>
      <img src="https://i.postimg.cc/W1jRkhb7/th-2.jpg" class="card-img" alt="Background Image">
      <div class="card-img-overlay">
        <h1 class="card-title">Welcome to the Task Management System</h1>
        <div class="card-body">
          

          <!-- Use v-cols and v-rows here -->
          <div class="container">
            <div class="row">
             
                <button type="button" class="btn btn-primary btn-block mb-2" @click="redirectToLogin">Login</button>
             
            
                <button type="button" class="btn btn-success btn-block" @click="redirectToRegistration">Registration</button>
                <p class="card-text1">If not signed in, register here:</p>
            </div>
          </div>

          <p class="card-text text-warning mt-3" v-if="showWarning">
            If not signed in, register here: One of these buttons should be clicked.
          </p>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  data() {
    return {
      showWarning: false,
    };
  },
  methods: {
    redirectToLogin() {
      // Redirect to the '/login' page
      this.$router.push('/login');
    },
    redirectToRegistration() {
      // Redirect to the '/RegistrationForm' page
      this.$router.push('/RegistrationForm');
    },
  },
};
</script>

<style scoped>
.container-fluid {
  background-color: #278cab69;
}

.card {
  max-width: 500px;
  width: 100%;
  border: none;
  border-radius: 15px;
}

.card-img {
  border-radius: 15px;
}

.card-title {
  text-align: center;
  margin-bottom: 1rem;
}

.card-text {
  text-align: center;
  margin-bottom: 1rem;
}
.card-text1 {
  text-align: center;
  margin-bottom: 1rem;
  color: red; /* Set text color to red */
  font-size: small; /* Set font size to small */
}

.btn-group {
  margin-top: 30px;
  display: flex;
  justify-content: center;
}

.text-warning {
  font-weight: bold;
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

/* Add any additional styling here */
</style>
