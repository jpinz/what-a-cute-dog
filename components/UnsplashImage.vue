<template>
  <div>
    <p v-if="$fetchState.pending">Fetching doggos...</p>
    <p v-else-if="$fetchState.error">An error occurred :(</p>
    <div v-else>
      <img :src="pictureUrl" />
      <p>{{ description }}</p>
      <br />
      <p v-if="picture.urls != null">
        <a :href="picture.user.links.html">{{ picture.user.name }}</a> - from
        <a href="unsplash.com">unsplash.com</a>
      </p>
      <button class="btn btn-blue" @click="$fetch">Refresh</button>
    </div>
  </div>
</template>
<script>
async function handleErrors(response) {
  if (!response.ok) {
    return await fetch("https://dog.ceo/api/breeds/image/random").then(
      (res) => res
    );
  }
  return response;
}

export default {
  data() {
    return {
      picture: {},
    };
  },
  computed: {
    pictureUrl: function () {
      if (this.picture.urls) {
        return this.picture.urls.raw;
      } else {
        return this.picture.message;
      }
    },
    description: function () {
      if (this.picture.description) {
        return this.picture.description;
      } else {
        return "A cute dog!";
      }
    },
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
