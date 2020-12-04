<template>
  <div class="detailPersonaje">
    {{ video.id }}
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      api_key: "563492ad6f91700001000001f702de05ef9942cd92919c9fa6f4561b",
      idVideo: this.$route.params.id,
      video: {}
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
    console.log(video);
  }
};
</script>

<style></style>
