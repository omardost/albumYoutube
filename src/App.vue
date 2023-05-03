<script setup>
import { ref } from "vue";
const arrayVideos = ref([]);
const urlYoutube = 'https://www.youtube.com/watch?v=';
const urlInput = ref('');

const loadVideo = async () => {
  console.log(urlInput.value);
  const idVideo = urlInput.value.replace(urlYoutube, '');

  const urlVideo = 'https://www.googleapis.com/youtube/v3/videos?id='.concat(idVideo).concat('&key=AIzaSyDSSkdkVVWm5XPz2TS6q2KiUbud3rlOU3k&part=snippet');

  const response = await fetch(urlVideo);
  const data = await response.json();
  arrayVideos.value.push(data);
  urlInput.value = '';
  console.log(arrayVideos);
}

const deleteVideo = () => {
  console.log('eliminado')
};

</script>

<template>
  <p>Añadir nuevo video</p>
  <input placeholder="Añadir" type="text" v-model="urlInput">
  <button @click="loadVideo">Añadir</button>

  <template v-for=" video  in  arrayVideos " :key="video.etag">

    <section class="button-container">
      <img :alt="video.etag" :src="video.items[0].snippet.thumbnails.medium.url">
      <a @click="deleteVideo">X</a>
    </section>

  </template>
</template>

<style scoped>
.button-container {
  display: inline-block;
  position: relative;
}

.button-container a{
  position: absolute;
  bottom: 9em;
  right: 0.4em;
  background-color: #000000;
  color: white;
  text-transform: uppercase;
  padding: 0.5em;
  cursor: pointer;
}
</style>

