<template>
  <div class="dashboard">
    <div class="containe">
      <div class="stats-grid">
        <div class="stat-card">
          <div class="stat-icon">
            <i class="fas fa-microphone"></i>
          </div>
          <div class="stat-info">
            <h3>Total Artists</h3>
            <p>{{ stats.artistCount }}</p>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon">
            <i class="fas fa-compact-disc"></i>
          </div>
          <div class="stat-info">
            <h3>Total Albums</h3>
            <p>{{ stats.albumCount }}</p>
          </div>
        </div>

        <div class="stat-card">
          <div class="stat-icon">
            <i class="fas fa-music"></i>
          </div>
          <div class="stat-info">
            <h3>Total Songs</h3>
            <p>{{ stats.songCount }}</p>
          </div>
        </div>
      </div>

      <div class="recent-activity">
        <h2>Recent Activity</h2>
        <div class="activity-list">
          <div
            v-for="activity in recentActivities"
            :key="activity.id"
            class="activity-item"
          >
            <div class="activity-icon">
              <i :class="activity.icon"></i>
            </div>
            <div class="activity-details">
              <p>{{ activity.description }}</p>
              <span>{{ activity.time }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
  
  <script>
export default {
  data() {
    return {
      stats: {
        artistCount: 0,
        albumCount: 0,
        songCount: 0,
      },
      recentActivities: [
        {
          id: 1,
          icon: "fas fa-plus",
          description: 'New artist "Arjit Singh" added',
          time: "2 minutes ago",
        },
        {
          id: 2,
          icon: "fas fa-upload",
          description: 'New album "Love Songs" uploaded',
          time: "1 hour ago",
        },
        {
          id: 3,
          icon: "fas fa-music",
          description: 'New song "Tum Hi Ho" added',
          time: "3 hours ago",
        },
      ],
    };
  },
  async created() {
    await this.fetchStats();
  },
  methods: {
    async fetchStats() {
      try {
        // Fetch dashboard statistics from your API
        const response = await fetch(
          "http://127.0.0.1:8000/api/dashboard/stats"
        );
        const data = await response.json();
        this.stats = data;
      } catch (error) {
        console.error("Error fetching stats:", error);
      }
    },
  },
};
</script>
  
  <style scoped>
.dashboard {
  padding: 20px;
  margin-left: 250px;
  /* margin-top: 60px; */
}

.containe{
  margin-top: 60px;
}
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  margin-bottom: 30px;
}

.stat-card {
  background: #282828;
  padding: 20px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  gap: 20px;
}

.stat-icon {
  width: 50px;
  height: 50px;
  background: #776bcc;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 24px;
}

.stat-info h3 {
  color: #b3b3b3;
  font-size: 16px;
  margin-bottom: 5px;
}

.stat-info p {
  color: white;
  font-size: 24px;
  font-weight: bold;
}

.recent-activity {
  background: #282828;
  padding: 20px;
  border-radius: 8px;
}

.recent-activity h2 {
  color: white;
  margin-bottom: 20px;
}

.activity-list {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.activity-item {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 10px;
  border-radius: 4px;
  background: #ffffff0a;
}

.activity-icon {
  width: 35px;
  height: 35px;
  background: #776bcc;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
}

.activity-details p {
  color: white;
  margin-bottom: 5px;
}

.activity-details span {
  color: #b3b3b3;
  font-size: 12px;
}
</style>
  