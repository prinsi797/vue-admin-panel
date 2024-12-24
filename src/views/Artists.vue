<template>
    <div class="artists-view">
      <ArtistList />
    </div>
  </template>
  
  <script>
  import ArtistList from '@/components/artists/ArtistList.vue'
  
  export default {
    components: {
      ArtistList
    }
  }
  </script>
  
  <style scoped>
  .artists-view {
    padding: 20px;
    padding-top: 90px; /* Account for fixed header */
  }
  </style>