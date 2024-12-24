<template>
  <div class="song-list">
    <div class="header">
      <h2>Songs</h2>
      <button @click="showCreateModal = true" class="btn-primary">
        Add New Songss
      </button>
    </div>

    <div class="songs-grid">
      <div v-for="song in songs" :key="song.id" class="song-card">
        <!-- <img :src="song.file_url" :alt="song.name" /> -->
        <audio controls :src="song.file_url" v-if="song.file_url"></audio>
        <h3>{{ song.name }}</h3>
        <p>Artist: {{ song.artist?.name }}</p>
        <p>Album: {{ song.album?.name }}</p>
        <div class="actions">
          <button @click="editSong(song)" class="btn-edit">
            <i class="fas fa-edit"></i>
          </button>
          <button @click="deleteSong(song.id)" class="btn-delete">
            <i class="fas fa-trash"></i>
          </button>
        </div>
      </div>
    </div>

    <!-- Create/Edit Modal -->
    <div v-if="showCreateModal" class="modal">
      <div class="modal-content">
        <h3 class="title">
          {{ editingSong ? "Edit Song" : "Create New Song" }}
        </h3>
        <form @submit.prevent="handleSubmit">
          <div class="form-group">
            <label>Name</label>
            <input v-model="formData.name" type="text" required />
          </div>

          <div class="form-group">
            <label>Artist</label>
            <select v-model="formData.artist_id" required>
              <option value="" disabled>Select an artist</option>
              <option
                v-for="artist in artists"
                :key="artist.id"
                :value="artist.id"
              >
                {{ artist.name }}
              </option>
            </select>
          </div>

          <div class="form-group">
            <label>Album</label>
            <select v-model="formData.album_id" required>
              <option value="" disabled>Select an album</option>
              <option v-for="album in albums" :key="album.id" :value="album.id">
                {{ album.name }}
              </option>
            </select>
          </div>

          <div class="form-group">
            <label>Cover Image</label>
            <input type="file" @change="handleImageUpload" accept="audio/*" />
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
              {{ editingSong ? "Update" : "Create" }}
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
      songs: [],
      artists: [],
      albums: [],
      showCreateModal: false,
      editingSong: null,
      formData: {
        name: "",
        artist_id: null,
        album_id: null,
        file_url: null,
      },
    };
  },

  created() {
    this.fetchSongs();
    this.fetchArtists();
    this.fetchAlbums();
  },

  methods: {
    async fetchSongs() {
      try {
        const response = await axios.get("http://127.0.0.1:8000/api/songs");
        this.songs = response.data;
      } catch (error) {
        console.error("Error fetching songs:", error);
      }
    },

    async fetchArtists() {
      try {
        const response = await axios.get("http://127.0.0.1:8000/api/artists");
        this.artists = response.data;
      } catch (error) {
        console.error("Error fetching artists:", error);
      }
    },

    async fetchAlbums() {
      try {
        const response = await axios.get("http://127.0.0.1:8000/api/albums");
        this.albums = response.data;
      } catch (error) {
        console.error("Error fetching albums:", error);
      }
    },

    handleImageUpload(event) {
      this.formData.file_url = event.target.files[0];
    },

    async handleSubmit() {
      try {
        const formData = new FormData();
        formData.append("name", this.formData.name);
        formData.append("artist_id", this.formData.artist_id);
        formData.append("album_id", this.formData.album_id);
        if (this.formData.file_url) {
          formData.append("file_url", this.formData.file_url);
        }

        if (this.editingSong) {
          await axios.put(
            `http://127.0.0.1:8000/api/songs/${this.editingSong.id}`,
            formData
          );
        } else {
          await axios.post("http://127.0.0.1:8000/api/songs", formData);
        }

        this.showCreateModal = false;
        this.fetchSongs();
        this.resetForm();
      } catch (error) {
        console.error("Error submitting form:", error);
      }
    },

    editSong(song) {
      this.editingSong = song;
      this.formData.name = song.name;
      this.formData.artist_id = song.artist?.id || null;
      this.formData.album_id = song.album?.id || null;
      this.showCreateModal = true;
    },

    async deleteSong(id) {
      if (confirm("Are you sure you want to delete this song?")) {
        try {
          await axios.delete(`http://127.0.0.1:8000/api/songs/${id}`);
          this.fetchSongs();
        } catch (error) {
          console.error("Error deleting song:", error);
        }
      }
    },

    resetForm() {
      this.formData = {
        name: "",
        artist_id: null,
        album_id: null,
        file_url: null,
      };
      this.editingSong = null;
    },
  },
};
</script>
<style scoped>
.song-list {
  padding: 20px;
  margin-left: 250px;
}
p {
  color: #ddd;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
}
h3 {
  color: #ddd;
}
.songs-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}

.song-card {
  background: #282828;
  border-radius: 8px;
  padding: 15px;
  text-align: center;
}

.song-card img {
  width: 150px;
  height: 150px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 10px;
}
select {
  width: 100%;
  height: 35px;
  border: 2px solid #f6efef;
}
.actions {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-top: 10px;
}

.title {
  color: #282828;
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
.songs-grid{
    grid-template-columns:repeat(auto-fill, minmax(300px, 1fr))
}
.btn-edit:hover {
  color: #776bcc;
}

.btn-delete:hover {
  color: #ff0000;
}
</style>
    