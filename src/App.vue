<script setup>
import { ref, computed } from "vue";

const questions = ref([
  {
    question: "If you want get reactive in Vue",
    answer: 1,
    options: ["Used value", "Used v-model", "Used v-bind"],
    selected: null,
  },
  {
    question: "What is Vuex",
    answer: 0,
    options: ["State manager", "New framework from Vue", "This CSS library"],
    selected: null,
  },
  {
    question: "What is Vue Router",
    answer: 1,
    options: [
      "Special Wi-Fi system for Vue developers",
      "Routing system in Vue",
      "Technology for communicate between Vue and React",
      "This NASA technology",
    ],
    selected: null,
  },
]);

const quizCompleted = ref(false);
const currentQuestion = ref(0);
const score = computed(() => {
  let value = 0;
  questions.value.map((q) => {
    if (q.selected == q.answer) {
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

const SetAnswer = (e) => {
  questions.value[currentQuestion.value].selected = e.target.value;
  e.target.value = null;
};

const NextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
  } else {
    quizCompleted.value = true;
  }
};
</script>

<template>
  <main class="app">
    <h2>The Quiz</h2>

    <section class="quiz" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question">{{ getCurrentQuestion.question }}</span>
        <span class="score">Score {{ score }}/{{ questions.length }}</span>
      </div>

      <div class="options">
        <label
          v-for="(option, index) in getCurrentQuestion.options"
          :key="index"
          :class="`option ${
            getCurrentQuestion.selected == index
              ? index == getCurrentQuestion.answer
                ? 'correct vibrate-3'
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
            :name="getCurrentQuestion.index"
            :value="index"
            v-model="getCurrentQuestion.selected"
            :disabled="getCurrentQuestion.selected"
            @change="SetAnswer"
          />
          <span>{{ option }}</span>
        </label>
      </div>
      <button @click="NextQuestion" :disabled="!getCurrentQuestion.selected">
        {{
          getCurrentQuestion.index == questions.length - 1
            ? "Finish"
            : getCurrentQuestion.selected == null
            ? "Select an option"
            : "Next Question"
        }}
      </button>
    </section>

    <section v-else>
      <h2>You finish</h2>
      <p>Score is {{ score }} / {{ questions.length }}</p>
    </section>
  </main>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Montserrat", sans-serif;
}

body {
  background-color: rgb(6, 49, 106);
  color: aliceblue;
}

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;
}

h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  margin-top: 2rem;
  background: rgb(28, 33, 78);
  padding: 0.5rem;
  border-radius: 0.5rem;
}

.quiz {
  background: rgb(28, 33, 78);
  padding: 1rem;
  width: 100%;
  max-width: 640px;
  border-radius: 1rem;
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question {
  color: white;
  font-size: 1.25rem;
}

.quiz-info .score {
  color: white;
  font-size: 1.25rem;
}

.options {
  margin-bottom: 1rem;
}

.option {
  padding: 1rem;
  display: block;
  background-color: rgb(6, 49, 106);
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover {
  background: rgb(26, 81, 153);
  box-shadow: 0px 0px 13px 1px rgb(75, 21, 149);
  transition: 0.5s;
}

.option.correct {
  background-color: rgb(26, 81, 153);
}

.option.wrong {
  background-color: rgba(255, 13, 13, 0.274);
}

.option:last-of-type {
  margin-bottom: 0;
}

.option.disabled {
  opacity: 0.5;
}

.option input {
  display: none;
}

button {
  border: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  color: white;
  text-transform: uppercase;
  font-size: 1rem;
  border-radius: 1.25rem;
  background: rgb(26, 81, 153);
}

.vibrate-3 {
  -webkit-animation: vibrate-3 0.5s linear infinite both;
  animation: vibrate-3 0.5s linear infinite both;
}

@-webkit-keyframes vibrate-3 {
  0% {
    -webkit-transform: translate(0);
    transform: translate(0);
  }
  10% {
    -webkit-transform: translate(-2px, -2px);
    transform: translate(-2px, -2px);
  }
  20% {
    -webkit-transform: translate(2px, -2px);
    transform: translate(2px, -2px);
  }
  30% {
    -webkit-transform: translate(-2px, 2px);
    transform: translate(-2px, 2px);
  }
  40% {
    -webkit-transform: translate(2px, 2px);
    transform: translate(2px, 2px);
  }
  50% {
    -webkit-transform: translate(-2px, -2px);
    transform: translate(-2px, -2px);
  }
  60% {
    -webkit-transform: translate(2px, -2px);
    transform: translate(2px, -2px);
  }
  70% {
    -webkit-transform: translate(-2px, 2px);
    transform: translate(-2px, 2px);
  }
  80% {
    -webkit-transform: translate(-2px, -2px);
    transform: translate(-2px, -2px);
  }
  90% {
    -webkit-transform: translate(2px, -2px);
    transform: translate(2px, -2px);
  }
  100% {
    -webkit-transform: translate(0);
    transform: translate(0);
  }
}
</style>
