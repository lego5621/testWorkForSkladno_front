<template>
  <div>
    <b-navbar variant="info">
      <b-navbar-brand to="/">NavBar</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <b-nav-item to="/">Главная</b-nav-item>
          <b-nav-item to="/admin">Админка</b-nav-item>
        </b-navbar-nav>

        <b-navbar-nav class="ml-auto">
          <b-nav-item v-if="!this.$auth.loggedIn" to="/login" right>
            Войти\Рег
          </b-nav-item>
          <b-nav-item v-if="this.$auth.loggedIn" right @click="logout"
            >Выйти</b-nav-item
          >
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>

    <b-container class="mt-3">
      <Nuxt />
    </b-container>
  </div>
</template>

<script>
export default {
  methods: {
    logout: async function () {
      await this.$auth.logout("local");
    },
  },
};
</script>
