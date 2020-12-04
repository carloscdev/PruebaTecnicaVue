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
    <h3 v-if="videos.length == 0">Sin Resultados ...</h3>
    <div v-else class="contenedor__general ancho">
      <div class="card" v-for="(video, index) in this.videos" :key="index">
        <Videos :video="video" />
      </div>
    </div>
    <div v-if="videos.length > 0" class="paginas ancho">
      <button v-if="numberPage > 1" @click.prevent="page(false)">
        Prev Page
      </button>
      <button @click.prevent="page(true)">Next Page</button>
    </div>
  </div>
</template>

<script>
import Videos from "../components/Videos";
const api_key = "563492ad6f91700001000001778d277b62924666b5926ae817c97666";
const headers = { Authorization: api_key };
export default {
  components: {
    Videos
  },
  data() {
    return {
      videos: [],
      titleDefault: "Populares",
      searchName: "",
      numberPage: 1
    };
  },
  computed: {
    validarBoton() {
      return this.searchName == "";
    },
    validarTitulo() {
      return this.titleDefault == "Populares"
        ? `Página : ${this.numberPage}`
        : "";
    }
  },
  async created() {
    const response = await fetch(`https://api.pexels.com/videos/popular`, {
      headers
    });
    const data = await response.json();
    this.videos = data.videos;
    console.log(this.videos);
    console.log(data);
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
    },
    async page(validar) {
      validar == true ? this.numberPage++ : this.numberPage--;
      const response = await fetch(
        `https://api.pexels.com/videos/popular?page=${this.numberPage}`,
        {
          headers
        }
      );
      const data = await response.json();
      this.videos = data.videos;
      console.log(this.videos);
    }
  }
};
</script>
