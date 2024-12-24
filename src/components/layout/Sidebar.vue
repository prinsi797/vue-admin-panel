<template>
    <div class="sidebar" :class="{ 'sidebar-collapsed': isCollapsed }">
      <div class="logo-container">
        <!-- <img src="@/assets/logo.png" alt="Logo" /> -->
        <h2 v-if="!isCollapsed">Music Admin</h2>
      </div>
      
      <nav>
        <router-link to="/dashboard">
          <i class="fas fa-home"></i>
          <span v-if="!isCollapsed">Dashboard</span>
        </router-link>
        
        <router-link to="/artists">
          <i class="fas fa-microphone"></i>
          <span v-if="!isCollapsed">Artists</span>
        </router-link>
        
        <router-link to="/albums">
          <i class="fas fa-compact-disc"></i>
          <span v-if="!isCollapsed">Albums</span>
        </router-link>
        
        <router-link to="/songs">
          <i class="fas fa-music"></i>
          <span v-if="!isCollapsed">Songs</span>
        </router-link>
      </nav>
      
      <button @click="toggleSidebar" class="collapse-btn">
        <i :class="isCollapsed ? 'fas fa-angle-right' : 'fas fa-angle-left'"></i>
      </button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        isCollapsed: false
      }
    },
    methods: {
      toggleSidebar() {
        this.isCollapsed = !this.isCollapsed;
      }
    }
  }
  </script>
  
  <style scoped>
  .sidebar {
    width: 250px;
    height: 100vh;
    background: #040404;
    color: white;
    transition: all 0.3s;
    position: fixed;
  }
  
  .sidebar-collapsed {
    width: 70px;
  }
  
  .logo-container {
    padding: 20px;
    display: flex;
    align-items: center;
  }
  
  .logo-container img {
    width: 40px;
    height: 40px;
  }
  
  nav {
    padding: 20px 0;
  }
  
  nav a {
    display: flex;
    align-items: center;
    padding: 15px 20px;
    color: #b3b3b3;
    text-decoration: none;
    transition: 0.3s;
  }
  
  nav a:hover, nav a.router-link-active {
    color: white;
    background: #282828;
  }
  
  nav a i {
    margin-right: 15px;
    width: 20px;
    text-align: center;
  }
  
  .collapse-btn {
    position: absolute;
    bottom: 20px;
    right: -15px;
    width: 30px;
    height: 30px;
    border-radius: 50%;
    background: #282828;
    border: none;
    color: white;
    cursor: pointer;
  }
  </style>
  