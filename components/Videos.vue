<template>
  <div class="videos">
    <nuxt-link :to="'/video/' + video.id">
      <div class="fotoPortada">
        <img :src="video.image" alt="Imagen" />
      </div>
    </nuxt-link>
  </div>
</template>

<script>
export default {
  props: {
    video: { type: Object, required: true }
  }
};
</script>

<style></style>
