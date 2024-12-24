<template>
    <div class="albums-view">
      <AlbumsList />
    </div>
  </template>
  
  <script>
  import AlbumsList from '@/components/albums/AlbumList.vue'
  
  export default {
    components: {
      AlbumsList
    }
  }
  </script>
  
  <style scoped>
  .albums-view {
    padding: 20px;
    padding-top: 90px; /* Account for fixed header */
  }
  </style>