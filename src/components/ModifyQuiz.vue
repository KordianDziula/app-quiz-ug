<template>
  <h3 class="mt-5">Quizy do wyboru</h3>
  
  <hr />
  
  <div class="list-group mt-5">
      <a href="#" 
         @click.stop="setQuiz(index)" 
         v-for="(quiz, index) in quizzes" 
         v-bind:key="index"
         class="list-group-item list-group-item-action">
         {{quiz.name}}
      </a>
  </div>
  
  <div v-if="quiz != null">
        <h3 class="mt-5">Nazwa</h3>
        <hr class="mb-5"/>

        <input v-model="quiz.name" type="text" class="input-text-quiz-name"/>

        <h3 class="mt-5">Pytania</h3>
        <hr />

        <div v-bind:key="questionIndex" class="mt-5" v-for="(question, questionIndex) in quiz.quizQuestions">
            <input type="text" v-model="quiz.quizQuestions[questionIndex].question" class="input-text-question"/>
            <button type="button" 
                          class="btn btn-dark button-delete"
                          @click.stop="deleteQuestion(questionIndex)">
                        USUN
            </button>

            <div v-bind:key="answerIndex" class="ml mt-3" v-for="(answer, answerIndex) in question.quizQuestionAnswers">
                <input v-model="quiz.quizQuestions[questionIndex].quizQuestionAnswers[answerIndex].isCorrect"
                       type="checkbox" 
                       name="answer.answer" />
                <label class="ml">
                  
                  <input type="text" v-model="quiz.quizQuestions[questionIndex].quizQuestionAnswers[answerIndex].answer">
                  
                  <button type="button" 
                          class="btn btn-dark button-delete"
                          @click.stop="deleteQuestionAnswer(questionIndex, answerIndex)">
                        USUN
                  </button>

                </label>
            </div>

            <button type="button" 
                    class="btn btn-dark mt-5" 
                    @click.stop="addQuestionAnswer(questionIndex)">
                DODAJ ODPOWIEDZ
            </button>

        </div>

        <button type="button" 
                class="btn btn-dark mt-5" 
                @click.stop="addQuestion()">
            DODAJ PYTANIE
        </button>
        
        <hr class="mt-5" />
        <button type="button" 
                class="btn btn-dark mt-5 mb-5" 
                @click.stop="saveChanges()">
            ZAPISZ
        </button>

  </div>
</template>

<script>
export default {
  name: 'ModifyQuiz',
  data(){
      return {
          quizzes: null,
          quiz: null,
      }
  },
  created(){
      fetch("https://localhost:44372/Quiz")
          .then(response => response.json())
          .then(data => this.quizzes = data);
  },
  methods: {
      setQuiz(quizId){
          this.quiz = JSON.parse(JSON.stringify(this.quizzes[quizId]));
      },
      addQuestionAnswer(questionIndex){
          this.quiz.quizQuestions[questionIndex].quizQuestionAnswers.push(
              {
                  quizQuestionId: this.quiz.quizQuestions[questionIndex].id,
                  answer: "",
                  isCorrect: false
              }
          )
      },
      deleteQuestionAnswer(questionIndex, answerIndex){
          this.quiz.quizQuestions[questionIndex].quizQuestionAnswers.splice(answerIndex, 1);
      },
      addQuestion(){
          this.quiz.quizQuestions.push(
              {
                  quizId: this.quiz.id,
                  question: "",
                  quizQuestionAnswers:[
                      {
                          answer: "",
                          isCorrect: false
                      }
                  ]
              }
          )
      },
      deleteQuestion(questionIndex){
        this.quiz.quizQuestions.splice(questionIndex, 1);
      },
      fetchDataAfterSave(response){
          alert(response);

          fetch("https://localhost:44372/Quiz")
          .then(response => response.json())
          .then(data => this.quizzes = data);
      },
      saveChanges(){
          fetch("https://localhost:44372/Quiz", { method: "PUT", body: JSON.stringify(this.quiz), headers: { 'Content-Type': 'application/json' }})
              .then(response => response.text())
              .then(text => this.fetchDataAfterSave(text))
      },
  }
}
</script>

<style scoped>
  .ml{
    margin-left: 1rem;
  }

  .input-text-quiz-name{
    width: 100%;
    font-size: 1.5rem;
  }

  .input-text-question{
    width: 70%;
    font-size: 1.5rem;
  }

  button{
    padding-left: 1.4rem;
    padding-right: 1.4rem;
    padding-top: 0.4rem;
    padding-bottom: 0.4rem;
    font-size: 1.1rem;
  }

  .button-delete{
    margin-top: 0;
    margin-left: 1rem;
  }

</style>
