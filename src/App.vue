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
  console.log(arrayVideos);
}

</script>

<template>
  <p>Añadir nuevo video</p>
  <input placeholder="Añadir" type="text" v-model="urlInput">
  <button @click=" loadVideo ">Añadir</button>

  <template v-for=" video  in  arrayVideos " :key="video.etag">
    <img :alt=" video.etag " :src=" video.items[0].snippet.thumbnails.medium.url ">

  </template>
</template>