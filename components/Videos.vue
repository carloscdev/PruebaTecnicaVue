<template>
  <div class="videos">
    <div class="fotoPortada">
      <nuxt-link :to="'/video/' + video.id">
        <img :src="video.image" alt="Imagen" />
      </nuxt-link>
    </div>
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
