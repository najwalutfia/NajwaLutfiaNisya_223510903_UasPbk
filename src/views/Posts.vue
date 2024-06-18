<template>
  <div class="post-container">
    <h1>Data Postingan</h1>
    <label for="user-select">Pilih User:</label>
    <select id="user-select" v-model="selectedUser" @change="fetchPosts">
      <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
    </select>
    <table>
      <thead>
        <tr>
          <th>Judul</th>
          <th>Isi</th>
          <th>Aksi</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="post in posts" :key="post.id">
          <td>{{ post.title }}</td>
          <td>{{ post.body }}</td>
          <td>
            <button @click="editPost(post.id)">Edit</button>
            <button @click="deletePost(post.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [],
      selectedUser: null,
      posts: []
    };
  },
  methods: {
    async fetchUsers() {
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        this.users = await response.json();
      } catch (error) {
        console.error('Failed to fetch users:', error);
      }
    },
    async fetchPosts() {
      if (this.selectedUser) {
        try {
          const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`);
          this.posts = await response.json();
        } catch (error) {
          console.error('Failed to fetch posts:', error);
        }
      }
    },
    async editPost(postId) {
      const newTitle = prompt("Enter new title:");
      const newBody = prompt("Enter new body:");
      if (newTitle && newBody) {
        try {
          const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${postId}`, {
            method: 'PUT',
            headers: {
              'Content-Type': 'application/json'
            },
            body: JSON.stringify({
              id: postId,
              title: newTitle,
              body: newBody,
              userId: this.selectedUser
            })
          });
          const updatedPost = await response.json();
          this.posts = this.posts.map(post => post.id === postId ? updatedPost : post);
        } catch (error) {
          console.error('Failed to edit post:', error);
        }
      }
    },
    async deletePost(postId) {
      if (confirm("Are you sure you want to delete this post?")) {
        try {
          await fetch(`https://jsonplaceholder.typicode.com/posts/${postId}`, {
            method: 'DELETE'
          });
          this.posts = this.posts.filter(post => post.id !== postId);
        } catch (error) {
          console.error('Failed to delete post:', error);
        }
      }
    }
  },
  created() {
    this.fetchUsers();
  }
};
</script>

<style>
.post-container {
  width: 800px;
  margin: 0 auto;
  padding: 20px;
  background-color: #e0f7fa;
  border: 1px solid #4db6ac;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  margin-top: 50px;
  margin-bottom: 20px;
}

h1 {
  color: #00796b;
  font-size: 32px !important;
  text-align: center;
  font-weight: bold !important;
  margin-bottom: -5px;
}

label {
  font-weight: bold;
  color: #004d40;
  margin-right: 5px;
}

select {
  padding: 8px;
  margin: 10px 0;
  border: 1px solid #4db6ac;
  border-radius: 4px;
  background-color: #ffffff;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th, td{
  border: 1px solid #4db6ac;
  padding: 12px;
  text-align: left;
}

th {
  background-color: #004d40;
  color: white;
}

td {
  background-color: #ffffff;
  color: #004d40;
}

button {
  padding: 8px 12px;
  margin-right: 5px;
  border: 1px solid white;
  border-radius: 5px;
  cursor: pointer;
  background-color: #4db6ac;
  color: #e0f7fa;
}

button:hover {
  background-color: #4db6ac;
  color: #ffffff;
}
</style>
