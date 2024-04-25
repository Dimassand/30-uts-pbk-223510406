<template>
  <div id="app">
      <h1> UCY & DIMAS CELL</h1>
      <input v-model="kegiatanBaru" @keyup.enter="tambahKegiatan" placeholder="Tambahkan nama kartu" class="input-text">
    <main>
      <input type="checkbox" v-model="filterSelesai" id="filterCheckbox">
      <label for="filterCheckbox">Tampilkan yang masih ada</label>
      <ul>
        <li v-for="kegiatan in filteredKegiatan" :key="kegiatan.id">
          <input type="checkbox" :checked="kegiatan.selesai" @change="toggleSelesai(kegiatan)" class="checkbox">
          <span :class="{ 'selesai': kegiatan.selesai }">{{ kegiatan.nama }}</span>
          <button @click="batalkanKegiatan(kegiatan.id)" v-if="!kegiatan.selesai" class="button">Hapus</button>
        </li>
      </ul>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      kegiatanList: [],
      kegiatanBaru: '',
      filterSelesai: false
    };
  },
  computed: {
    filteredKegiatan() {
      if (this.filterSelesai) {
        return this.kegiatanList.filter(kegiatan => !kegiatan.selesai);
      } else {
        return this.kegiatanList;
      }
    }
  },
  methods: {
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

span.selesai {
  text-decoration: line-through;
}

.input-text {
  width: 70%;
  margin-right: 10px;
  padding: 5px;
}

.checkbox {
  margin-right: 5px;
}

.button {
  margin-left: auto;
}
</style>
