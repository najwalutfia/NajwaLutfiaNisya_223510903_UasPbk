<template>
  <div class="todos-container">
    <h1>Daftar Kegiatan</h1>
    <input type="text" v-model="newActivity.name" placeholder="Tambahkan kegiatan baru">
    <div class="datetime-container">
      <input id="datetime" type="datetime-local" v-model="newActivity.dateTime">
    </div>
    <button @click="addActivity">Tambah</button>
    <table>
      <thead>
        <tr>
          <th>Aktivitas</th>
          <th>Tanggal & Jam</th>
          <th>Status</th>
          <th>Aksi</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(activity, index) in activities" :key="index">
          <td>{{ activity.name }}</td>
          <td>{{ formatDate(activity.dateTime) }}</td>
          <td>
            <input type="checkbox" v-model="activity.completed">
            <span :class="{ 'completed': activity.completed }">{{ activity.completed ? 'Selesai' : 'Belum Selesai' }}</span>
          </td>
          <td><button @click="removeActivity(index)">Hapus</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newActivity: {
        name: '',
        dateTime: ''
      },
      activities: []
    };
  },
  methods: {
    addActivity() {
      if (this.newActivity.name.trim() !== '' && this.newActivity.dateTime.trim() !== '') {
        this.activities.push({ 
          name: this.newActivity.name, 
          dateTime: this.newActivity.dateTime, 
          completed: false 
        });
        this.newActivity.name = '';
        this.newActivity.dateTime = '';
      }
    },
    removeActivity(index) {
      this.activities.splice(index, 1);
    },
    formatDate(dateTime) {
      const options = { year: 'numeric', month: 'short', day: 'numeric', hour: 'numeric', minute: 'numeric' };
      return new Date(dateTime).toLocaleDateString('en-US', options);
    }
  }
};
</script>

<style scoped>
.todos-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: #e0f7fa;
  padding: 15px;
  color: #00796b;
  width: 800px;
  border-radius: 10px;
  margin: 0 auto;
  margin-top: 50px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border: 1px solid #4db6ac;
}

h1 {
  color: #004d40;
  font-size: 32px;
  margin-bottom: -15px;
  font-weight: bold;
}

input[type="text"],
input[type="datetime-local"],
button {
  margin-top: 10px;
  padding: 10px;
  border: 1px solid #004d40;
  border-radius: 5px;
}

button {
  background-color: #004d40;
  color: white;
  cursor: pointer;
}

button:hover {
  background-color: #00332e;
}

table {
  margin-top: 20px;
  border-collapse: collapse;
  width: 100%;
}

th, td {
  border: 1px solid #00796b;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #004d40;
  color: white;
}

.completed {
  text-decoration: line-through;
}

.datetime-container {
  margin-top: 10px;
}

.datetime-container label {
  display: block;
  margin-bottom: 5px;
}
</style>
