<template>

  <input type="text" v-on:input="searchRequest" placeholder="Musique">

</template>

<script setup>

import {ref} from "vue";
import axios from "axios";

const tracks = ref({});
const search = ref("");

const searchRequest = (elem) => {

  const token = localStorage.getItem('spotify_token');
  const search = elem.target.value;

  console.log(search, token);

  if (search.length > 3) {
    fetch(`https://api.spotify.com/v1/search?q=${search}&type=track&limit=4`, {
      method: 'GET',
      headers: {
        'Authorization': `Bearer ${token}`
      }
    }).then((response) => {
      return response.json();
    }).then((data) => {
      tracks.value = {};
      data.tracks.items.forEach((track) => {
        axios.get(track.href, {
          headers: {
            'Authorization': `Bearer ${token}`
          }
        }).then((response) => {
          tracks.value[response.data.id] = response.data;
        }).catch((error) => {
          console.log(error);
        })
      });

      console.log('Tracks', tracks.value);

    }).catch((error) => {
      console.log(error);
    });
  }

};

</script>

<style scoped>

</style>