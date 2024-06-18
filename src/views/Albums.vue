<template>
  <div class="albums">
    <div class="album-table">
      <div class="album-header">
        <span>Judul Album</span>
      </div>
      <div v-for="album in albums" :key="album.id" class="album-row">
        <router-link :to="'/albums/' + album.id">{{ album.title }}</router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, onMounted } from 'vue';
import { useAlbumsStore } from '../store/album';

export default defineComponent({
  setup() {
    const albums = ref([]);
    const albumsStore = useAlbumsStore();

    onMounted(async () => {
      await albumsStore.fetchAlbums();
      albums.value = albumsStore.albums;
    });

    return {
      albums,
    };
  },
});
</script>

<style scoped>
.albums {
  max-width: 75%;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
  color: #e0f7fa;
  font-size: 20px;
  background-size: cover;
}

.album-table {
  display: grid;
  grid-template-columns: 1fr;
  gap: 10px;
  border-radius: 5px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.album-header {
  display: flex;
  justify-content: center;
  font-weight: bold;
  border-bottom: 2px solid #004d40;
  padding: 15px;
  background-color: #004d40;
  color: #e0f7fa;
}

.album-row {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  border: 1px solid #004d40;
  border-top: none;
  transition: background-color 0.3s, transform 0.3s;
  background-color: #00796b;
  min-height: 50px;
}

.album-row a {
  text-decoration: none;
  color: #e0f7fa;
  font-size: 1.1rem;
}

.album-row:hover {
  background-color: #004d40;
  transform: translateY(-2px);
  border-color: #004d40;
}
</style>
