<template>
  <div>
    <p v-if="$fetchState.pending">Fetching doggos...</p>
    <p v-else-if="$fetchState.error">An error occurred :(</p>
    <div v-else>
      <img :src="picture.urls.raw" />
      <button class="btn btn-blue" @click="$fetch">Refresh</button>
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
      `https://api.unsplash.com/photos/random?client_id=${this.$config.accessKey}&query=dog`
    )
      .then(handleErrors)
      .then((res) => res.json());
  },
};
</script>
