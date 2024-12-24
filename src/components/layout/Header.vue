<template>
  <header class="header">
    <div class="search-bar">
      <i class="fas fa-search"></i>
      <input type="text" placeholder="Search..." v-model="searchQuery" />
    </div>
    
    <div class="user-menu">
      <div class="notifications">
        <i class="fas fa-bell"></i>
        <span class="notification-badge" v-if="notificationCount">{{ notificationCount }}</span>
      </div>
      
      <div class="user-profile" @click="toggleUserMenu">
        <div class="avatar">
          <i class="fas fa-user"></i>
        </div>
        <span>Admin</span>
        <i class="fas fa-chevron-down"></i>
        
        <div class="dropdown-menu" v-if="showUserMenu">
          <a href="#" @click.prevent="handleProfile">Profile</a>
          <a href="#" @click.prevent="handleSettings">Settings</a>
          <a href="#" @click.prevent="handleLogout">Logout</a>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Header',
  data() {
    return {
      searchQuery: '',
      notificationCount: 3,
      showUserMenu: false
    }
  },
  methods: {
    toggleUserMenu() {
      this.showUserMenu = !this.showUserMenu;
    },
    handleProfile() {
      this.showUserMenu = false;
    },
    handleSettings() {
      this.showUserMenu = false;
    },
    async handleLogout() {
      try {
        // Clear token and user data
        localStorage.removeItem('token');
        localStorage.removeItem('token_type');
        
        // Remove axios default header
        delete axios.defaults.headers.common['Authorization'];
        
        // Redirect to login
        this.$router.push('/login');
        
        this.showUserMenu = false;
      } catch (error) {
        console.error('Logout error:', error);
      }
    }
  }
}
</script>

<style scoped>
.header {
  background: #282828;
  height: 70px;
  padding: 0 30px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: fixed;
  top: 0;
  right: 0;
  left: 250px;
  z-index: 100;
}

.search-bar {
  position: relative;
  width: 300px;
}

.search-bar input {
  width: 100%;
  padding: 8px 35px;
  border-radius: 20px;
  border: none;
  background: #ffffff1a;
  color: white;
}

.search-bar i {
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: #b3b3b3;
}

.user-menu {
  display: flex;
  align-items: center;
  gap: 20px;
}

.notifications {
  position: relative;
  color: white;
  cursor: pointer;
}

.notification-badge {
  position: absolute;
  top: -8px;
  right: -8px;
  background: #776bcc;
  color: white;
  border-radius: 50%;
  width: 18px;
  height: 18px;
  font-size: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.user-profile {
  display: flex;
  align-items: center;
  gap: 10px;
  color: white;
  cursor: pointer;
  position: relative;
}

.avatar {
  width: 35px;
  height: 35px;
  border-radius: 50%;
  /* background: #1db954; */
  background: #776bcc;
  display: flex;
  align-items: center;
  justify-content: center;
}

.dropdown-menu {
  position: absolute;
  top: 100%;
  right: 0;
  background: #282828;
  border-radius: 4px;
  padding: 8px 0;
  min-width: 150px;
  margin-top: 10px;
}

.dropdown-menu a {
  display: block;
  padding: 8px 20px;
  color: white;
  text-decoration: none;
}

.dropdown-menu a:hover {
  background: #ffffff1a;
}

input::placeholder {
  color: #b3b3b3;
}
</style>