<template>
  <form @submit.prevent>
    <h1>Создание поста</h1>
    <my-input v-model="post.title"
           placeholder="Название"/>
    <my-input v-model="post.body"
           placeholder="Описание"/>
    <my-button style="align-self: flex-end; margin-top: 15px;"
               @click="createPost">Создать пост
    </my-button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      post: {
        title: "",
        body: "",
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = Date.now();
      this.$emit("create", this.post);
      this.post = {
        title: "",
        body: "",
      }
    },
  }
}
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}
</style>