<template>
  <div>
    <b-tabs content-class="mt-3">
      <b-tab title="Логин">
        <b-form-input
          v-model="loginData.email"
          placeholder="Email"
          class="mt-2 mb-2"
        ></b-form-input>
        <b-form-input
          v-model="loginData.password"
          placeholder="password"
          class="mt-2 mb-2"
        ></b-form-input>
        <b-button class="mt-2 mb-2" v-on:click="login">Логин</b-button>
      </b-tab>

      <b-tab title="Регистрация">
        <b-form-input
          v-model="registerData.email"
          placeholder="Email"
          class="mt-2 mb-2"
        ></b-form-input>
        <b-form-input
          v-model="registerData.name"
          placeholder="text"
          class="mt-2 mb-2"
        ></b-form-input>
        <b-form-input
          v-model="registerData.password"
          placeholder="password"
          class="mt-2 mb-2"
        ></b-form-input>
        <b-form-input
          v-model="registerData.c_password"
          placeholder="password"
          class="mt-2 mb-2"
        ></b-form-input>
        <b-button class="mt-2 mb-2" v-on:click="register">Регистрация</b-button>
      </b-tab>
    </b-tabs>

    <p v-if="this.error">
      {{ error }}
    </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginData: {
        email: "qwerty@gmail.com",
        password: "zcxvcbvn12",
      },

      registerData: {
        name: "Oleerwerg",
        email: "qwerzterwery@gmal.com",
        password: "zcxvcbvn12",
        c_password: "zcxvcbvn12",
      },

      error: null,
    };
  },
  methods: {
    async login() {
      try {
        await this.$auth.loginWith("local", { data: this.loginData });

        this.$router.push("/admin");
      } catch (e) {
        this.error = e.response.data.message;
      }
    },

    async register() {
      // После регистрации получаем токен, и его сразу можно прикрепить в заголовок HTTP, но,
      // либо я проглядел, либо у nuxt auth нет возможности вручную прикрипить токен + возможны баги,
      // решил пойти немного другим путем

      try {
        await this.$axios.$post("api/register", this.registerData);

        await this.$auth.loginWith("local", {
          data: {
            email: this.registerData.email,
            password: this.registerData.password,
          },
        });

        this.$router.push("/");
      } catch (e) {
        this.error = e.response.data.message;
      }
    },
  },
};
</script>
