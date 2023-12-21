<template>
  <div>
    <nav class="navbar navbar-expand-lg bg-body-tertiary">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <NuxtLink to="/" class="nav-link">Home</NuxtLink>
        </li>
        <li class="nav-item">
          <NuxtLink to="/login" class="nav-link">Login</NuxtLink>
        </li>
        <li class="nav-item">
          <NuxtLink to="/RegistrationForm" class="nav-link">Register</NuxtLink>
        </li>
        <li class="nav-item">
          <NuxtLink to="/task" class="nav-link">All Tasks</NuxtLink>
        </li>
        <li class="nav-item">
          <NuxtLink to="/myTask" class="nav-link">My Tasks</NuxtLink>
        </li>
        <li class="nav-item">
          <NuxtLink to="/allusers" class="nav-link">All Users</NuxtLink>
        </li>
      <li class="nav-item">
          <NuxtLink to="/mydetails" class="nav-link">My Details</NuxtLink>
        </li>
      </ul>
      <div class="logout-btn">
          <button @click="logout" class="btn btn-danger">Logout</button>
        </div>
    </div>
  </div>
</nav>
    </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'AppHeader',
  methods: {
    async logout() {
      try {
        const response = await axios.get('http://localhost:8091/User/logout',{
          withCredentials: true,
        });
        console.log(response.data);
        this.$router.push('/login');
      } catch (error) {
        console.error('Error during logout:', error);
      }
    },
  },

}
</script>

<style>

</style>