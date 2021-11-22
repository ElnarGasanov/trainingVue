<!--корневое приложение APP-->

<template>
  <!--ЗДЕСЬ МЫ ПИШЕМ РАЗМЕТКУ-->
  <main class="app">
    <form>
      <h4>Создание поста</h4>
      <input class="input" type="text" placeholder="Название">
      <input class="input" type="text" placeholder="Описание">
      <button class="btn" @click="createPost">Создать пост</button>
    </form>
    <div class="post" v-for="post in posts">
      <div><strong>Название:</strong>{{ post.title }}</div>
      <div><strong>Описание:</strong>{{ post.body }}</div>
    </div>
  </main>
</template>

<script>
//ЗДЕСЬ описываем логику, создаём функции, объявляем данные
//важный момент в этой секции по default мы должны экспортировать объект{}
export default {
  data() {
    return {
      posts: [
        {id: 1, title: "JavaScript 1", body: "Описание поста 1"},
        {id: 2, title: "JavaScript 2", body: "Описание поста 2"},
        {id: 3, title: "JavaScript 3", body: "Описание поста 3"},
      ],
    }
  },
  methods: {
    createPost:{

    }
  }
}
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app{
  padding: 20px;
}

.post {
  margin-top: 15px;
  padding: 15px;
  border: 2px solid teal;
}

form{
  display: flex;
  flex-direction: column;
}

.input{
  width: 100%;
  border: 1px solid teal;
  padding: 10px 15px;
  margin-top: 15px;
}
.btn{
  align-self: flex-end;
  margin-top: 15px;
  padding: 10px 20px;
  background: none;
  color: teal;
  border: 1px solid teal;
  cursor: pointer;
}
</style>
