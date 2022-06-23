<template>
  <h3 class="mt-5">Quizy do wyboru</h3>
  
  <hr />
  
  <div class="list-group mt-5">
      <a href="#" 
         @click.stop="setQuestions(index)" 
         v-for="(quiz, index) in quizzes" 
         v-bind:key="index"
         class="list-group-item list-group-item-action">
         {{quiz.name}}
      </a>
  </div>
  
  <div v-if="currentQuiz != null">
        <h3 class="mt-5">Pytania</h3>
        <hr />

        <div class="mt-5" v-bind:key="questionIndex" v-for="(question, questionIndex) in questions">
            <p class="question">{{questionIndex + 1}}. {{question.question}}</p>

            <div v-bind:key="answerIndex" class="ml" v-for="(answer, answerIndex) in question.quizQuestionAnswers">

                <input v-model="questions[questionIndex].quizQuestionAnswers[answerIndex].isChecked"
                       type="checkbox" 
                       name="answer.answer">

                <label class="ml">
                  {{answer.answer}}
                </label>

            </div>
        </div>

        <button type="button" 
                class="btn btn-dark mt-5" 
                @click.stop="checkAnswers()">
          SPRAWDŹ
        </button>

  </div>
</template>

<script>
export default {
  name: 'ResolveQuiz',
  data(){
      return {
          quizzes: null,
          questions: null,
          currentQuiz: null,
      }
  },
  created(){
      fetch("https://localhost:44372/Quiz")
          .then(response => response.json())
          .then(data => this.quizzes = data);
  },
  methods: {
      setQuestions(quizId){
          this.currentQuiz = quizId;
          this.questions = this.quizzes[quizId].quizQuestions;
          

        for(let i = 0; i < this.questions.length; i++){
            for(let j = 0; j < this.questions[i].quizQuestionAnswers.length; j++){
                this.questions[i].quizQuestionAnswers[j] = { ...this.questions[i].quizQuestionAnswers[j], ...{ isChecked: false} };
            }
        }
      },
      checkAnswers(){
          let points = 0;
      
          for(let i = 0; i < this.questions.length; i++){
              for(let j = 0; j < this.questions[i].quizQuestionAnswers.length; j++){
                let questionAnswer = this.questions[i].quizQuestionAnswers[j]

                if(questionAnswer.isChecked & questionAnswer.isCorrect){
                    points += 1;
                }

                if(questionAnswer.isChecked & !questionAnswer.isCorrect){
                    points -= 1;
                }

              }
          }
      
          alert("Liczba uzyskanych punktow: " + points);
      }
  }
}
</script>

<style scoped>
  .ml{
    margin-left: 1rem;
  }

  button{
    padding-left: 1.4rem;
    padding-right: 1.4rem;
    padding-top: 0.4rem;
    padding-bottom: 0.4rem;
    font-size: 1.1rem;
  }
</style>
