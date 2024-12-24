<template>
    <div class="songs-view">
      <SongsList />
    </div>
  </template>
  
  <script>
  import SongsList from '@/components/songs/SongsList.vue'
  
  export default {
    components: {
        SongsList
    }
  }
  </script>
  
  <style scoped>
  .songs-view {
    padding: 20px;
    padding-top: 90px; /* Account for fixed header */
  }
  </style>