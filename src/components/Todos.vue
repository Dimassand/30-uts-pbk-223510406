<template>
    <div>
      <slot name="input"></slot>
      <ul>
        <li v-for="kegiatan in filteredKegiatan" :key="kegiatan.id">
          <input type="checkbox" :checked="kegiatan.selesai" @change="toggleSelesai(kegiatan)" class="checkbox">
          <span :class="{ 'selesai': kegiatan.selesai }">{{ kegiatan.nama }}</span>
          <button @click="batalkanKegiatan(kegiatan.id)" v-if="!kegiatan.selesai" class="button">Hapus</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Todos',
    props: {
      kegiatanList: Array,
      filterSelesai: Boolean
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
      toggleSelesai(kegiatan) {
        this.$emit('toggle-selesai', kegiatan);
      },
      batalkanKegiatan(id) {
        this.$emit('batalkan-kegiatan', id);
      }
    }
  };
  </script>
  
  <style scoped>
  .selesai {
    text-decoration: line-through;
  }
  .checkbox {
    margin-right: 5px;
  }
  .button {
    margin-left: auto;
  }
  </style>
  
