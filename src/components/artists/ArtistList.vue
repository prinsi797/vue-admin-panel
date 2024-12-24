<template>
    <div class="artist-list">
      <div class="header">
        <h2>Artists</h2>
        <button @click="showCreateModal = true" class="btn-primary">
          Add New Artist
        </button>
      </div>
  
      <div class="artists-grid">
        <div v-for="artist in artists" :key="artist.id" class="artist-card">
          <img :src="artist.profile_picture" :alt="artist.name" />
          <h3>{{ artist.name }}</h3>
          <div class="actions">
            <button @click="editArtist(artist)" class="btn-edit">
              <i class="fas fa-edit"></i>
            </button>
            <button @click="deleteArtist(artist.id)" class="btn-delete">
              <i class="fas fa-trash"></i>
            </button>
          </div>
        </div>
      </div>
  
      <!-- Create/Edit Modal -->
      <div v-if="showCreateModal" class="modal">
        <div class="modal-content">
          <h3 class="title">{{ editingArtist ? "Edit Artist" : "Create New Artist" }}</h3>
          <form @submit.prevent="handleSubmit">
            <div class="form-group">
              <label>Name</label>
              <input v-model="formData.name" type="text" required />
            </div>
  
            <div class="form-group">
              <label>Profile Picture</label>
              <input type="file" @change="handleImageUpload" accept="image/*" />
            </div>
  
            <div class="modal-actions">
              <button
                type="button"
                @click="showCreateModal = false"
                class="btn-secondary"
              >
                Cancel
              </button>
              <button type="submit" class="btn-primary">
                {{ editingArtist ? "Update" : "Create" }}
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    data() {
      return {
        artists: [],
        showCreateModal: false,
        editingArtist: null,
        formData: {
          name: "",
          profile_picture: null,
        },
      };
    },
  
    created() {
      this.fetchArtists();
    },
  
    methods: {
      async fetchArtists() {
        try {
          const response = await axios.get("http://127.0.0.1:8000/api/artists");
          this.artists = response.data;
        } catch (error) {
          console.error("Error fetching artists:", error);
        }
      },
  
      handleImageUpload(event) {
        this.formData.profile_picture = event.target.files[0];
      },
  
      async handleSubmit() {
        try {
          const formData = new FormData();
          formData.append("name", this.formData.name);
          if (this.formData.profile_picture) {
            formData.append("profile_picture", this.formData.profile_picture);
          }
  
          if (this.editingArtist) {
            await axios.put(
              `http://127.0.0.1:8000/api/artists/${this.editingArtist.id}`,
              formData
            );
          } else {
            await axios.post("http://127.0.0.1:8000/api/artists", formData);
          }
  
          this.showCreateModal = false;
          this.fetchArtists();
          this.resetForm();
        } catch (error) {
          console.error("Error submitting form:", error);
        }
      },
  
      editArtist(artist) {
        this.editingArtist = artist;
        this.formData.name = artist.name;
        this.showCreateModal = true;
      },
  
      async deleteArtist(id) {
        if (confirm("Are you sure you want to delete this artist?")) {
          try {
            await axios.delete(`http://127.0.0.1:8000/api/artists/${id}`);
            this.fetchArtists();
          } catch (error) {
            console.error("Error deleting artist:", error);
          }
        }
      },
  
      resetForm() {
        this.formData = {
          name: "",
          profile_picture: null,
        };
        this.editingArtist = null;
      },
    },
  };
  </script>
  
  <style scoped>
  .artist-list {
    padding: 20px;
    margin-left: 250px;
  }
  
  .header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 30px;
  }
  h3{
    color: #ddd;
  }
  .title{
    color: #282828;
  }
  .artists-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 20px;
  }
  
  .artist-card {
    background: #282828;
    border-radius: 8px;
    padding: 15px;
    text-align: center;
  }
  
  .artist-card img {
    width: 150px;
    height: 150px;
    border-radius: 50%;
    object-fit: cover;
    margin-bottom: 10px;
  }
  
  .actions {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin-top: 10px;
  }
  
  .modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  
  .modal-content {
    background: white;
    padding: 20px;
    border-radius: 8px;
    width: 100%;
    max-width: 500px;
  }
  
  .form-group {
    margin-bottom: 15px;
  }
  
  .form-group label {
    display: block;
    margin-bottom: 5px;
  }
  
  .form-group input {
    width: 100%;
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
  }
  
  .modal-actions {
    display: flex;
    justify-content: flex-end;
    gap: 10px;
    margin-top: 20px;
  }
  
  .btn-primary {
    background: #776bcc;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .btn-secondary {
    background: #282828;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
  }
  
  .btn-edit,
  .btn-delete {
    background: none;
    border: none;
    color: white;
    cursor: pointer;
    padding: 5px;
  }
  
  .btn-edit:hover {
    color: #776bcc;
  }
  
  .btn-delete:hover {
    color: #ff0000;
  }
  </style>
  