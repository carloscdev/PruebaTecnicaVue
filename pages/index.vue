<template>
  <div class="contenedor">
    <div class="contenedor__hero">
      <div class="contenedor__hero--texto">
        <h1>Pexels Videos</h1>
        <input
          v-model.trim="searchName"
          type="text"
          placeholder="Buscar video por nombre"
          @keydown.enter.prevent="searchByNameValidation"
        />
        <br />
        <button @click="searchByNameValidation" :disabled="validarBoton">
          {{ validarBusqueda }}
        </button>
        <br />
        <hr />
        <div class="sugerencias">
          <div v-for="t in tagPopular" :key="t.id">
            <input
              type="radio"
              :name="t.id"
              :id="t.id"
              :value="t.id"
              v-model="searchFast"
            />
            <label :for="t.id">{{ t.id }}</label>
          </div>
        </div>
      </div>
    </div>
    <h2 class="ancho">{{ titleDefault }}</h2>
    <h3 v-if="videos.length == 0">Sin Resultados ...</h3>
    <div v-else class="contenedor__general ancho">
      <div class="card" v-for="(video, index) in this.videos" :key="index">
        <Videos :video="video" />
      </div>
    </div>
    <strong class="numberPage ancho">{{ validarTitulo }}</strong>
    <div
      v-if="videos.length > 0 && titleDefault === 'Populares'"
      class="paginas ancho"
    >
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
const api_url = "https://api.pexels.com/videos";
export default {
  components: {
    Videos
  },
  data() {
    return {
      videos: [],
      titleDefault: "Populares",
      searchName: "",
      numberPage: 1,
      searchFast: "",
      loading: false,
      tagPopular: [{ id: "Tigers" }, { id: "Nature" }, { id: "People" }]
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
    },
    validarBusqueda() {
      return this.loading === true ? "Buscando ..." : "Buscar";
    }
  },
  async created() {
    const response = await fetch(`${api_url}/popular`, {
      headers
    });
    const data = await response.json();
    this.videos = data.videos;
  },
  watch: {
    searchFast() {
      if (this.searchFast !== "") {
        this.searchName = this.searchFast;
        this.searchByName();
      }
    }
  },
  methods: {
    async searchByName() {
      this.loading = true;
      const response = await fetch(
        `${api_url}/search?query=${this.searchName}&per_page=15`,
        { headers }
      );
      const data = await response.json();
      this.videos = data.videos;
      this.titleDefault = this.searchName;
      this.searchName = "";
      this.loading = false;
    },
    searchByNameValidation() {
      if (this.searchName === "") {
        return alert("Necesitas rellenar el campo para buscar");
      }
      if (this.searchFast !== "") {
        this.searchFast = "";
      }

      this.searchByName();
    },
    async page(validar) {
      validar == true ? this.numberPage++ : this.numberPage--;
      const response = await fetch(
        `${api_url}/popular?page=${this.numberPage}`,
        {
          headers
        }
      );
      const data = await response.json();
      this.videos = data.videos;
    }
  }
};
</script>
