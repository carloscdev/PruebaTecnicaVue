<template>
  <div class="detailVideo ancho">
    <h2>Video: {{ video.id }}</h2>
    <div class="containerVideo">
      <iframe
        :src="nVideo"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
      ></iframe>
    </div>
    <p>
      Duración : {{ video.duration }} Seg. <br />
      Creador : {{ creador }} <br />
    </p>
    <nuxt-link to="/" class="button">
      Volver a inicio
    </nuxt-link>
  </div>
</template>
<script>
export default {
  data() {
    return {
      api_key: "563492ad6f91700001000001778d277b62924666b5926ae817c97666",
      idVideo: this.$route.params.id,
      video: {},
      nVideo: "",
      creador: ""
    };
  },
  async created() {
    const headers = { Authorization: this.api_key };
    const response = await fetch(
      `https://api.pexels.com/videos/videos/${this.idVideo}`,
      { headers }
    );
    const video = await response.json();
    this.video = video;
    this.nVideo = video.video_files[0].link;
    this.creador = video.user.name;
  }
};
</script>

<style></style>
