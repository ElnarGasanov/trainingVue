<!--корневое приложение APP-->

<template>
  <!--ЗДЕСЬ МЫ ПИШЕМ РАЗМЕТКУ-->
  <main class="app">
    <h1>Страница с постами</h1>
    <my-button style="margin-top: 10px; margin-bottom: 20px" @click="showDialog">Добавить пост</my-button>
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost"/>
    </my-dialog>

    <post-list @remove="removePost"
        :posts="posts"/>
  </main>
</template>

<script>
//ЗДЕСЬ описываем логику, создаём функции, объявляем данные
//важный момент в этой секции по default мы должны экспортировать объект{}
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";

export default {
  components:{
    PostForm, PostList,
  },
  data() {
    return {
      posts: [
        {id: 1, title: "JavaScript 1", body: "Описание поста 1"},
        {id: 2, title: "JavaScript 2", body: "Описание поста 2"},
        {id: 3, title: "JavaScript 3", body: "Jписание поста 3"},
      ],
      dialogVisible: false,
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post){
      this.posts = this.posts.filter(p => p.id !== post.id);
    },
    showDialog(){
      this.dialogVisible = true;
    }
  }
}
</script>

<style scoped>
/*ЗДЕСЬ ПРОПИСЫВАТЬ СТИЛИ*/
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app{
  padding: 20px;
}
</style>
