<template>
  <div>
    <h3><NuxtLink to="news/create" class="card-link"> Создать новую </NuxtLink></h3>
    <b-card
      class="mb-3"
      v-for="(post, index) in this.posts"
      v-bind:key="post.id"
    >
      <h1 v-if="post.title">
        {{ post.title }}
      </h1>
      <NuxtLink
        :to="{ name: 'news-id', params: { id: post.id } }"
        class="card-link"
      >
        Читать полностью
      </NuxtLink>
      <NuxtLink
        :to="{ name: 'news-update-id', params: { id: post.id } }"
        class="card-link"
      >
        Редактировать
      </NuxtLink>
      <a href="#" class="card-link" @click="deleteArticle(post.id, index)">Удалить</a>
    </b-card>
  </div>
</template>

<script>
export default {
  name: "Admin",
  data: function () {
    return {
      posts: [],
      error: "",
    };
  },

  methods: {
    async deleteArticle(id, index) {
      try {
        await this.$axios.delete(`api/user/article/${id}`);
        this.posts.splice(index, 1);

        this.$router.push("/admin");
      } catch (e) {
        this.error = e.response.data.message;
      }
    },
  },

  async fetch() {
    try {
      let post = await this.$axios.$get("/api/user/article");
      this.posts = post.data;
    } catch (e) {
      this.$router.push("/login");
    }
  },
};
</script>
