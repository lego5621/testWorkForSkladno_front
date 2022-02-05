<template>
  <div>
    <h2>Редактирование статьи</h2>
    <div>
      <b-form-input
        v-model="data.title"
        type="text"
        placeholder="Название"
        class="mt-3"
      ></b-form-input>

      <client-only>
        <VueEditor
          id="editor"
          useCustomImageHandler
          @image-added="handleImageAdded"
          v-model="data.text"
          class="mt-3"
          :editor-toolbar="customToolbar"
        />
      </client-only>

      <p class="mt-2">Дата публикации</p>
      <b-form-input
        v-model="data.published_at"
        type="datetime-local"
      ></b-form-input>

      <b-button variant="danger" class="mt-3" @click="method">Сохранить</b-button>
    </div>
    {{ this.error }}
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      customToolbar: [
        ["bold", "italic", "underline"],
        [{ list: "ordered" }, { list: "bullet" }],
        ["image"],
      ],

      data: {
        text: "",
        title: "",
        published_at: "",
      },
      error: "",
    };
  },

  methods: {
    handleImageAdded(file, Editor, cursorLocation, resetUploader) {
      var formData = new FormData();
      formData.append("image", file);

      this.$axios
        .post("api/uploadImage", formData)
        .then((result) => {
          const url = result.data;
          Editor.insertEmbed(
            cursorLocation,
            "image",
            "http://127.0.0.1:8000/" + url
          );
          resetUploader();
        })
        .catch((err) => {
          console.log(err);
        });
    },

    async method() {
      try {
        await this.$axios.$put(
          `api/user/article/${this.$route.params.id}`,
          this.data
        );

        this.$router.push("/admin");
      } catch (e) {
        this.error = e.response.data.message;
      }
    },
  },

  async fetch() {
    let data = await this.$axios.$get(`api/article/${this.$route.params.id}`);
    this.data = data.data;
    this.data.published_at = data.data.published_at.replace(/\s/g, "T");
  },
};
</script>
