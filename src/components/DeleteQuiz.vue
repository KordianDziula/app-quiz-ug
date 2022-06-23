<template>
  <h3 class="mt-5">Quizy do wyboru</h3>
  
  <hr />
  
  <div class="list-group mt-5">
      <a href="#" 
         @click.stop="deleteQuiz(quiz)" 
         v-for="(quiz, index) in quizzes" 
         v-bind:key="index"
         class="list-group-item list-group-item-action">
         {{quiz.name}}
      </a>
  </div>
</template>

<script>
export default {
  name: 'DeleteQuiz',
  data(){
      return {
          quizzes: null,
      }
  },
  created(){
      fetch("https://localhost:44372/Quiz")
          .then(response => response.json())
          .then(data => this.quizzes = data);
  },
  methods: {
      fetchDataAfterSave(text){
          alert(text);

          fetch("https://localhost:44372/Quiz")
          .then(response => response.json())
          .then(data => this.quizzes = data);
      },
      deleteQuiz(quiz){
          fetch("https://localhost:44372/Quiz/" + quiz.id, { method: "DELETE" })
              .then(response => response.text())
              .then(text => this.fetchDataAfterSave(text))
      },
  }
}
</script>

<style scoped>

</style>
