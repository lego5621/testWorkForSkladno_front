<template>
  <div>
    <b-card class="mb-3" v-for="post in this.posts" v-bind:key="post.id">
      <h1 v-if="post.title">
        {{ post.title }}
      </h1>
      <NuxtLink
        :to="{ name: 'news-id', params: { id: post.id } }"
        class="card-link"
      >
        Читать полностью
      </NuxtLink>
    </b-card>
  </div>
</template>

<script>
export default {
  name: "Index",
  data: function () {
    return {
      posts: [],
    };
  },
  async fetch() {
    let post = await this.$axios.$get("api/article");
    this.posts = post.data;
  },
};
</script>

<style>
img {
  width: 100%;
}
</style>
