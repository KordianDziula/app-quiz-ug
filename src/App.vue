<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark py-4 sticky-top">
      <div class="container">
          <a class="navbar-brand" href="#"><i class="fas fa-paper-plane"></i> QUIZ APP</a>
      </div>
  </nav>

  <div class="container mt-5">
    <ul class="nav justify-content-center">
        <li class="nav-item">
          <a @click.stop="setPage('resolve-quiz')" class="nav-link" href="#">Rozwiaz Quiz</a>
        </li>
        <li class="nav-item">
          <a @click.stop="setPage('modify-quiz')" class="nav-link" href="#">Modyfikuj Quiz</a>
        </li>
        <li class="nav-item">
          <a @click.stop="setPage('add-quiz')" class="nav-link" href="#">Dodaj Quiz</a>
        </li>
        <li class="nav-item">
          <a @click.stop="setPage('delete-quiz')" class="nav-link" href="#">Usun Quiz</a>
        </li>
    </ul>

    <ResolveQuiz v-if="currentPage === 'resolve-quiz'"/>
    <ModifyQuiz v-if="currentPage === 'modify-quiz'"/>
    <AddQuiz v-if="currentPage === 'add-quiz'"/>
    <DeleteQuiz v-if="currentPage == 'delete-quiz'"/>
  </div>

</template>

<script>

import ResolveQuiz from './components/ResolveQuiz.vue';
import ModifyQuiz from './components/ModifyQuiz.vue';
import AddQuiz from './components/AddQuiz.vue';
import DeleteQuiz from './components/DeleteQuiz.vue';

export default {
  name: 'App',
  components: {
    ResolveQuiz,
    ModifyQuiz,
    AddQuiz,
    DeleteQuiz
},
  data(){
    return {
      currentPage: null
    }
  },
  methods: {
    setPage(page){
      this.currentPage = page;
    }
  }
}

</script>

<style>
  * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
  }
  html {
      font-size: 62.5%;
  }

  body {
      font-size: 1.6rem;
  }

  .navbar-brand{
    font-size: 2rem;
    font-weight: bold;
    text-transform: none;
  }

</style>
