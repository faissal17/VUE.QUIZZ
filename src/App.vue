<script setup>
import { ref, computed } from "vue";
const questions = ref([
  {
    question: "What is Vue?",
    answer: 0,
    options: ["A framework", "A library", "A type of hat"],
    selected: null,
  },
  {
    question: "What is Vuex used for?",
    answer: 2,
    options: ["Eating a delicious snack", "Viewing things", "State management"],
    selected: null,
  },
  {
    question: "What is Vue Router?",
    answer: 1,
    options: [
      "An ice cream maker",
      "A routing library for Vue",
      "Burger sauce",
    ],
    selected: null,
  },
]);
const quizCompleted = ref(false);
const currentQuestion = ref(0);
const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected != null && q.answer == q.selected) {
      console.log("correct");
      value++;
    }
  });
  return value;
});
const getCurrentQuestion = computed(() => {
  let question = questions.value[currentQuestion.value];
  question.index = currentQuestion.value;
  return question;
});

const setAnswer = (e) => {
  questions.value[currentQuestion.value].selected = e.target.value;
  e.target.value = null;
};
const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length[0]) {
    currentQuestion.value++;
  } else {
    quizCompleted.value = true;
  }
};
</script>
<!-- end of first script -->
<script>
export default {
  data() {
    return {
      show: true,
      name: "",
    };
  },
  methods: {
    toggle() {
      this.show = !this.show;
    },
  },
};
</script>

<template>
  <!-- div centred -->
  <div v-if="show" class="division">
    <input
      class="inp"
      type="text"
      placeholder="enter you name"
      v-model="name"
      required
    />
    <button type="submit" v-on:click="toggle" class="botn">
      start the quiz
    </button>
  </div>
  <!-- div centred -->
  <div v-else class="quizsec">
    <mian class="app">
      <h1>Welcom {{ name }}</h1>
      <section class="quiz">
        <div class="quiz-info">
          <span class="question">{{ getCurrentQuestion.question }}</span>
          <span class="score">Score {{ score }}/{{ questions.length }}</span>
        </div>

        <div class="options">
          <label
            v-for="(option, index) in getCurrentQuestion.options"
            :for="'option' + index"
            :class="`option ${
              getCurrentQuestion.selected == index
                ? index == getCurrentQuestion.answer
                  ? 'correct'
                  : 'wrong'
                : ''
            } ${
              getCurrentQuestion.selected != null &&
              index != getCurrentQuestion.selected
                ? 'disabled'
                : ''
            }`"
          >
            <input
              type="radio"
              :id="'option' + index"
              :name="getCurrentQuestion.index"
              :value="index"
              v-model="getCurrentQuestion.selected"
              :disabled="getCurrentQuestion.selected"
              @change="SetAnswer"
            />
            <span>{{ option }}</span>
          </label>
        </div>
      </section>
    </mian>
  </div>
</template>
