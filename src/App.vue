<!-- src/App.vue -->
<template>
  <div id="app">
    <!-- Only show Sidebar and Header if not on login route and user is logged in -->
    <template v-if="!isLoginRoute && isAuthenticated">
      <Sidebar />
      <div class="main-content" :class="{ 'sidebar-collapsed': isSidebarCollapsed }">
        <Header />
        <router-view></router-view>
      </div>
    </template>
    
    <!-- Show only router-view without layout if on login route or not authenticated -->
    <template v-else>
      <router-view></router-view>
    </template>
  </div>
</template>

<script>
import Sidebar from './components/layout/Sidebar.vue'
import Header from './components/layout/Header.vue'

export default {
  name: 'App',
  components: {
    Sidebar,
    Header
  },
  data() {
    return {
      isSidebarCollapsed: false
    }
  },
  computed: {
    // Check if current route is login route
    isLoginRoute() {
      return this.$route.path === '/login'
    },
    // Check if user is authenticated
    isAuthenticated() {
      // Replace this with your actual authentication logic
      // For example, checking Vuex store or localStorage
      return localStorage.getItem('token') !== null
    }
  }
}
</script>