<template>

  <form @submit.prevent="submit">

    <input type="text" placeholder="Client ID" name="clientid">

    <input type="text" placeholder="Client Secret" name="clientsecret">

    <button type="submit">Refresh Token</button>

  </form>

</template>

<script setup>
  import axios from "axios";

  const submit = (e) => {
    console.log('submit')

    const clientid = e.target.clientid.value
    const clientsecret = e.target.clientsecret.value

    const config = {
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
      }
    }

    axios.post('https://accounts.spotify.com/api/token', {
      grant_type: 'client_credentials',
      client_id: clientid,
      client_secret: clientsecret
    },
    config).then((response) => {
      localStorage.setItem('spotify_token', response.data.access_token)
    }).catch((error) => {
      alert('Error')
    });

  }
</script>

<style scoped>

</style>