<template>
  <div>
    <p v-if="$fetchState.pending">Fetching mountains...</p>
    <p v-else-if="$fetchState.error">An error occurred :(</p>
    <div v-else>
      <h1>Cute Dog</h1>
      <img :src="picture.urls.raw" />
      <button @click="$fetch">Refresh</button>
    </div>
  </div>
</template>
<script>
function handleErrors(response) {
    if (!response.ok) {
        throw Error(response.statusText);
    }
    return response;
}

export default {
  data() {
    return {
      picture: {},
    };
  },
  async fetch() {
    this.picture = await fetch(
      `https://api.unsplash.com/photos/random?client_id=${process.env.ACCESS_KEY}&query=dog`
    ).then(handleErrors)
    .then((res) => res.json());
  },
};
</script>
