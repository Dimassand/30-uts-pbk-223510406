<template>
  <div id="app">
    <h1>UCY & DIMAS CELL</h1>
    <header>
      <button @click="currentView = 'todos'" :class="{ active: currentView === 'todos' }">Todos</button>
      <button @click="currentView = 'posts'" :class="{ active: currentView === 'posts' }">Post</button>
    </header>

    <main v-if="currentView === 'todos'">
      <Todos
        :kegiatanList="kegiatanList"
        :filterSelesai="filterSelesai"
        @toggle-selesai="toggleSelesai"
        @batalkan-kegiatan="batalkanKegiatan"
      >
        <template #input>
          <input v-model="kegiatanBaru" @keyup.enter="tambahKegiatan" placeholder="Tambahkan nama kartu" class="input-text">
          <input type="checkbox" v-model="filterSelesai" id="filterCheckbox">
          <label for="filterCheckbox">Tampilkan yang masih ada</label>
        </template>
      </Todos>
    </main>

    <main v-else>
      <label for="userSelect">Pilih User:</label>
      <select v-model="selectedUser" @change="fetchPosts" id="userSelect">
        <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
      </select>
      <ul>
        <li v-for="post in posts" :key="post.id">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
        </li>
      </ul>
    </main>
  </div>
</template>

<script>
import Todos from './Todos.vue';

export default {
  name: 'App',
  components: {
    Todos
  },
  data() {
    return {
      currentView: 'todos', // to track the current view
      kegiatanList: [],
      kegiatanBaru: '',
      filterSelesai: false,
      users: [],
      posts: [],
      selectedUser: null
    };
  },
  methods: {
    async fetchUsers() {
      const response = await fetch('https://jsonplaceholder.typicode.com/users');
      this.users = await response.json();
    },
    async fetchPosts() {
      if (this.selectedUser) {
        const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`);
        this.posts = await response.json();
      }
    },
    tambahKegiatan() {
      if (this.kegiatanBaru.trim() !== '') {
        this.kegiatanList.push({ id: Date.now(), nama: this.kegiatanBaru.trim(), selesai: false });
        this.kegiatanBaru = '';
      }
    },
    batalkanKegiatan(id) {
      this.kegiatanList = this.kegiatanList.filter(kegiatan => kegiatan.id !== id);
    },
    toggleSelesai(kegiatan) {
      kegiatan.selesai = !kegiatan.selesai;
    }
  },
  mounted() {
    this.fetchUsers();
  }
};
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin-top: 20px;
  background-color: white;
  margin:auto;
  width: 500px;
  padding:25px;
  color:black;
}
h1 {
  margin: 0;
}
header {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}
button {
  margin: 0 10px;
  padding: 10px;
  cursor: pointer;
}
button.active {
  font-weight: bold;
  text-decoration: underline;
}
main {
  margin-top: 20px;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  margin-bottom: 10px;
  display: flex;
  align-items: center;
}
.input-text {
  width: 70%;
  margin-right: 10px;
  padding: 5px;
}
</style>
