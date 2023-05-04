<script setup>
import { ref } from "vue";
const arrayVideos = ref([]);
const urlYoutube = 'https://www.youtube.com/watch?v=';
const urlInput = ref('');
const videoDelete = ref(0);
const viewVideo = ref();

const loadVideo = async () => {
  console.log(urlInput.value);
  const idVideo = urlInput.value.replace(urlYoutube, '');

  const urlVideo = 'https://www.googleapis.com/youtube/v3/videos?id='.concat(idVideo).concat('&key=AIzaSyDSSkdkVVWm5XPz2TS6q2KiUbud3rlOU3k&part=snippet');

  const response = await fetch(urlVideo);
  const data = await response.json();
  arrayVideos.value.push(data);
  urlInput.value = '';
  console.log(data);

}

const deleteAdd = (data) => {
  videoDelete.value = data;
};

const deleteAction = () => {
  arrayVideos.value.splice(videoDelete.value, 1);
}

const viewAction = (data) => {
  viewVideo.value = data;
  console.log(data);
}

</script>

<template>
  <h1>Añadir nuevo video</h1>

  <div class="input-group mb-3">
    <input type="text" class="form-control" placeholder="Añadir" aria-label="Añadir" aria-describedby="button-addon2"
      v-model="urlInput">
    <button class="btn btn-primary" type="button" id="button-addon2" @click="loadVideo">Añadir</button>
  </div>

  <!-- lista de videos -->
  <template v-for="(video, index) in  arrayVideos " :key="video.etag">

    <section class="button-container">
      <img :alt="video.etag" :src="video.items[0].snippet.thumbnails.medium.url" @click="viewAction(video)"
        data-bs-toggle="modal" data-bs-target="#exampleModal1">
      <a @click="deleteAdd(index)" data-bs-toggle="modal" data-bs-target="#exampleModal" title="Eliminar">X</a>
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

  <!-- Modal -->
  <div class="modal fade" id="exampleModal1" tabindex="-1" aria-labelledby="exampleModalLabel1" aria-hidden="true">
    <div class="modal-dialog modal-xl">
      <div class="modal-content">
        <div class="modal-header">
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <template v-if="viewVideo">
            <div style="display: flex; align-items: center">

              <div style="cursor: pointer;" title="Reproducir">
                <a :href="urlYoutube.concat(viewVideo.items[0].id)">

                  <img :alt="viewVideo.etag" :src="viewVideo.items[0].snippet.thumbnails.medium.url">
                </a>
              </div>
              <div style="padding: 10px;">
                <h1>{{ viewVideo.items[0].snippet.title }}</h1>
                <p>{{ viewVideo.items[0].snippet.description }}</p>

              </div>
            </div>
          </template>
        </div>

      </div>
    </div>
  </div>
</template>

<style scoped>
.button-container {
  display: inline-block;
  position: relative;
  padding: 10px;
}

.button-container a {
  position: absolute;
  bottom: 9em;
  right: 1.4em;
  background-color: #000000;
  color: white !important;
  text-transform: uppercase;
  padding: 0.2em 0.5em;
  cursor: pointer;
}
</style>

