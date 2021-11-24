<!--корневое приложение APP-->

<template>
  <!--ЗДЕСЬ МЫ ПИШЕМ РАЗМЕТКУ-->
  <main class="app">
    <h1>Страница с постами</h1>
    <my-input style="width: 100%" placeholder="Поиск по названию..." v-model="searchQuery"/>
    <div class="app__btns">
      <my-button @click="showDialog">Добавить пост</my-button>
      <my-select v-model="selectedSort" :options="sortOptions"></my-select>
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost"/>
    </my-dialog>
    <post-list v-if="!isPostLoading" @remove="removePost" :posts="sortedAndSearchedPosts"/>
    <div v-else>Идёт загрузка...</div>
    <div class="page__wrapper">
      <div v-for="pageNumber in totalPages"
           :key="pageNumber" class="page"
           :class="{
             'current-page': page === pageNumber
           }"
           @click="changePage(pageNumber)"
      >{{ pageNumber }}
      </div>
    </div>
  </main>
</template>

<script>
//ЗДЕСЬ описываем логику, создаём функции, объявляем данные
//важный момент в этой секции по default мы должны экспортировать объект{}
import PostForm from "@/components/PostForm";
import PostList from "@/components/PostList";
import axios from "axios";

export default {
  components: {
    PostForm, PostList,
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostLoading: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 4,
      totalPages: 0,
      sortOptions: [
        {value: 'title', name: 'По названию'},
        {value: 'body', name: 'По описанию'},
        {value: 'id', name: 'По порядку(не работает)'},
      ]
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id);
    },
    showDialog() {
      this.dialogVisible = true;
    },
    changePage(pageNumber) {
      this.page = pageNumber;
    },
    async fetchPosts() {
      try {
        this.isPostLoading = true;
        const response = await axios.get("https://jsonplaceholder.typicode.com/posts", {
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
        this.posts = response.data;
      } catch (e) {
        alert("ERROR")
      } finally {
        this.isPostLoading = false;
      }
    }
  },
  mounted() {
    this.fetchPosts()
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]));
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  },
  watch:{
    page() {
      this.fetchPosts()
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

.app {
  padding: 20px;
}

.app__btns {
  display: flex;
  justify-content: space-between;

  margin: 15px 0;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid black;
  padding: 10px;
  cursor: pointer;
}

.current-page {
  border: 2px solid teal;
}
</style>
