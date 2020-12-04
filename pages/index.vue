<template>
  <div class="contenedor">
    <div class="contenedor__hero">
      <div class="contenedor__hero--texto">
        <h1>Pexels Videos</h1>
        <input
          v-model.trim="searchName"
          type="text"
          placeholder="Buscar video por nombre"
          @keydown.enter.prevent="searchByName"
        />
        <br />
        <button @click="searchByName" :disabled="validarBoton">Buscar</button>
      </div>
    </div>
    <h2 class="ancho">{{ titleDefault }}</h2>
    <div class="contenedor__general ancho">
      <div class="card" v-for="(video, index) in this.videos" :key="index">
        <Videos :video="video" />
      </div>
    </div>
  </div>
</template>

<script>
import Videos from "../components/Videos";
const api_key = "563492ad6f91700001000001f702de05ef9942cd92919c9fa6f4561b";
const headers = { Authorization: api_key };
export default {
  components: {
    Videos
  },
  data() {
    return {
      videos: [],
      titleDefault: "Populares",
      searchName: ""
    };
  },
  computed: {
    validarBoton() {
      return this.searchName == "";
    }
  },
  async created() {
    const response = await fetch(`https://api.pexels.com/videos/popular`, {
      headers
    });
    const data = await response.json();
    this.videos = data.videos;
    console.log(this.videos);
  },
  methods: {
    async searchByName() {
      const response = await fetch(
        `https://api.pexels.com/videos/search?query=${this.searchName}&per_page=15`,
        { headers }
      );
      const data = await response.json();
      this.videos = data.videos;
      this.titleDefault = this.searchName;
      this.searchName = "";
      console.log(this.videos);
    }
  }
};
</script>

<style lang="scss">
@import "../static/styles/style.scss";
</style>
