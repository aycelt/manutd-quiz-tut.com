<script setup>
import { computed, ref } from 'vue';

const questions = ref([
  {
    question: 'what is Manchester United Football club known for',
    answer: 0,
    options: [
      'A football playing Institution',
      'A basketball playing Institution',
      'A team of badminton playing individuals'
    ],
    selected: null
  },
  {
    question: 'who was Manchester United Top Scorer last season',
    answer: 2,
    options: [
      'Rasmus Winthur Hojllund',
      'Alejandro Garnacho',
      'Dr. Marcus Rashford'
    ],
    selected: null
  },
  {
    question: 'Which of these Premier League goalkeepers won the Golden glove in 22/23 PL Season',
    answer: 2,
    options: [
      'Hugo Lloris',
      'Allison Becker',
      'David De Gea',
      'Aaron Ramsdale'
    ],
    selected: null
  },
  {
    question: 'What position did Manchester United finish last season',
    answer: 1,
    options: [
      '19th Position',
      '3rd Place',
      '4th Place',
      '10th Position'
    ],
    selected: null
  },
  {
    question: 'who was Manchester United Manager last season',
    answer: 3,
    options: [
      'Mikel Arteta',
      'Thomas Frank',
      'Julien Lopetugei',
      'Erik Ten Hag'
    ],
    selected: null
  }
])

const quizcompleted = ref(false)
const currentquestion = ref(0)
const score = computed(() => {
  return questions.value.reduce((totalScore, q) => {
    if (parseInt(q.selected) === q.answer) {
      totalScore++;
    }
    return totalScore;
  }, 0)
})

const getcurrentquestion = computed(()=>{
  let question = questions.value[currentquestion.value]
  question.index = currentquestion.value
  return question
})

const setanswer = evt =>{
  questions.value[currentquestion.value].selected = evt.target.value
  evt.target.value = null
}

const nextquestion = ()=>{
  if(currentquestion.value < questions.value.length - 1){
    currentquestion.value++
  } else{
    quizcompleted.value = true
  }
}

</script>

<template>
    <main class="app">
        <h1>The Quiz</h1>
        <section class="quiz" v-if="!quizcompleted">
        <div class="quiz-info">
            <span class="question">{{ getcurrentquestion.question }}</span>
            <span class="score">{{ score }} / {{ questions.length }}</span>
        </div>

        <div class="options">

          <label
        v-for="(option, index) in getcurrentquestion.options"
        :key="index"
        :class="`option ${
          getcurrentquestion.selected == null ? '' :

          getcurrentquestion.answer == getcurrentquestion.selected &&
          getcurrentquestion.selected == index  ? 'correct' : 
          
          getcurrentquestion.answer != getcurrentquestion.selected &&
          getcurrentquestion.selected == index  ? 'wrong' : 'disabled'
          }`"
        
      >
      <!-- :class="{
          correct:  ,
          wrong: index !== getcurrentquestion.answer && getcurrentquestion.selected === index,
          disabled: getcurrentquestion.selected !== null
        }" -->
        <input
          type="radio"
          :name="getcurrentquestion.index"
          :value="index"
          :disabled="getcurrentquestion.selected !== null"
          v-model="getcurrentquestion.selected"
          @change="setanswer"
        >
        {{ option }}
      </label>

    </div>

    <button
    @click="nextquestion"
    :disabled="!getcurrentquestion.selected">
          {{ 
                getcurrentquestion.index === questions.length - 1 
                  ? 'finish'
                  : getcurrentquestion.selected == null 
                    ? 'select an option'
                    : 'Next question'
          
          }}
   </button>

      </section>

      <section v-else>
        <h2>You have finished the quiz</h2>
        <p>your score is {{ score }}/ {{ questions.length }}</p>
      </section>

    </main>

</template>

<style>
 * {
margin: 0;
padding: 0;
box-sizing: border-box;
font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
} 

body{
  background-color: #271c36;
  color: #fff;
}

.app{
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;
}

h1{
  font-size: 2rem;
  margin-bottom: 2rem;
}

.quiz{
  background-color: #382a4b;
  padding: 1rem;
  width: 100%;
  max-width: 640px;
  border-radius: 0.5rem;
}

.quiz-info{
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question{
  color: #8f8f8f;
  font-size: 1.25rem;
}

.quiz-info .score{
  color: #fff;
  font-size: 1.25rem;
}

.options{
  margin-bottom: 1rem;
}

.option{
  display: block;
  padding: 1rem;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover{
  background-color: #2d213f;
}

.option.correct{
  background-color: green;
}

.option.wrong{
  background-color: red;
}

.option:last-of-type{
  margin-bottom: 0;

}

.option.disabled{
  opacity: 0.5;
}

.option.input{
  display: none;
  background: red;
}

button{
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem 1.5rem;
  background: green;
  color:#2d213f;
  text-transform: uppercase;
  font-size: 1.25rem;
  border-radius: 0.5rem;

}

button:disabled{
  opacity: 0.5;
}

</style>

c:\Users\user\Documents\Tuts\tutorial-appp
