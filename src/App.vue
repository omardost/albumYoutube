<script setup>
import { ref } from "vue";
const arrayVideos = ref([]);
const urlYoutube = 'https://www.youtube.com/watch?v=';
const urlInput = ref('');
const videoDelete = ref(0);

const loadVideo = async () => {
  console.log(urlInput.value);
  const idVideo = urlInput.value.replace(urlYoutube, '');

  const urlVideo = 'https://www.googleapis.com/youtube/v3/videos?id='.concat(idVideo).concat('&key=AIzaSyDSSkdkVVWm5XPz2TS6q2KiUbud3rlOU3k&part=snippet');

  const response = await fetch(urlVideo);
  const data = await response.json();
  arrayVideos.value.push(data);
  urlInput.value = '';

}

const deleteAdd = (data) => {
  videoDelete.value = data;
};

const deleteAction = () => {
  arrayVideos.value.splice(videoDelete.value, 1);
}

</script>

<template>
  <p>Añadir nuevo video</p>
  <input placeholder="Añadir" type="text" v-model="urlInput">
  <button type="button" class="btn btn-primary" @click="loadVideo">Añadir</button>

  <!-- lista de videos -->
  <template v-for="(video, index) in  arrayVideos " :key="video.etag">

    <section class="button-container">
      <img :alt="video.etag" :src="video.items[0].snippet.thumbnails.medium.url">
      <a @click="deleteAdd(index)" data-bs-toggle="modal" data-bs-target="#exampleModal">X</a>
    </section>

  </template>

  <!-- Modal -->
  <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="exampleModalLabel">¿Seguro que quieres eliminar este video?</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>

        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
          <button type="button" class="btn btn-primary" @click="deleteAction" data-bs-dismiss="modal">Eliminar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.button-container {
  display: inline-block;
  position: relative;
}

.button-container a {
  position: absolute;
  bottom: 9em;
  right: 0.4em;
  background-color: #000000;
  color: white !important;
  text-transform: uppercase;
  padding: 0.2em 0.5em;
  cursor: pointer;
}
</style>

