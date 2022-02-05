<template>
  <div v-if="this.post.data">
    <h1>{{ this.post.data.title }}</h1>
    <div>
      <div v-html="this.post.data.text"></div>
    </div>
    <p>опубликованно {{ this.post.data.published_at }}</p>
  </div>
</template>

<script>
export default {
  name: "NuxtTutorial",
  data: function () {
    return {
      post: [],
    };
  },
  async fetch() {
    this.post = await this.$axios.$get(`api/article/${this.$route.params.id}`);
  },
};
</script>
