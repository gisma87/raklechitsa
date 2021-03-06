<template>
  <form class="quiz" v-if="!thanksForSubmit">
    <h3 class="quiz__title">{{ currentQuestion.title }}</h3>

    <p class="quiz__question">
      {{ currentQuestion.questionMain }}
      <span
        class="quiz__question quiz__question_type_additional"
        v-if="currentQuestion.questionAdditional"
        >{{ currentQuestion.questionAdditional }}</span
      >
    </p>
    <quiz-input
      class="quiz__input"
      type="text"
      v-model="answer"
      placeholder="Напишите тут"
    />

    <div class="quiz__submit">
      <div class="quiz__buttons">
        <quiz-button
          @btnClick="prevQuestion"
          class="quiz__button quiz__button_direction_back"
          :disabled="isFirstQuestion"
          >Назад</quiz-button
        >
        <quiz-button
          :disabled="isAnswerEmpty"
          @btnClick="btnAction"
          class="quiz__button quiz__button_direction_next"
          >{{ isLastQuestion ? 'Отправить' : 'Далее' }}</quiz-button
        >
      </div>

      <p class="quiz__policy" v-if="isLastQuestion">
        Нажимая на кнопку «Отправить», вы даете согласие на
        <a class="quiz__policy-link" href="/policy" target="_blank"
          >обработку персональных данных</a
        >
      </p>
    </div>
  </form>
  <div v-else class="quiz__thanks">
    <h3 class="quiz__title quiz__title_type_thanks">
      Спасибо, что приняли участие!
    </h3>
    <quiz-button
      @btnClick="setNewQuiz"
      class="quiz__button quiz__button_direction_next"
      >Закрыть</quiz-button
    >
  </div>
</template>

<script>
import Button from '@/components/ui/Button';
import Input from '@/components/ui/Input';

export default {
  components: {
    'quiz-button': Button,
    'quiz-input': Input,
  },

  data() {
    return {
      answer: '',
      thanksForSubmit: false,
    };
  },

  computed: {
    currentQuestion() {
      const { quiz } = this.$store.state;
      const { currentQuestion, questions } = quiz;
      return questions[currentQuestion];
    },

    initialAnswer() {
      const { quiz } = this.$store.state;
      const { currentQuestion, answers } = quiz;
      return answers[currentQuestion] || '';
    },

    isFirstQuestion() {
      const { quiz } = this.$store.state;
      const { currentQuestion } = quiz;
      if (currentQuestion === 1) {
        return true;
      }
      return false;
    },

    isLastQuestion() {
      const { quiz } = this.$store.state;
      const { questions, currentQuestion } = quiz;
      const questionsLength = Object.keys(questions).length;
      if (currentQuestion === questionsLength) {
        return true;
      }
      return false;
    },

    isAnswerEmpty() {
      if (this.answer === '') {
        return true;
      }
    },

    btnAction() {
      if (this.isLastQuestion) {
        return this.submitQuiz;
      }
      return this.nextQuestion;
    },
  },

  methods: {
    async nextQuestion() {
      await this.$store.dispatch('quiz/NEXT_QUESTION', {
        answer: this.answer,
      });
      this.answer = this.initialAnswer;
    },
    async prevQuestion() {
      await this.$store.dispatch('quiz/PREV_QUESTION');
      this.answer = this.initialAnswer;
    },

    submitQuiz() {
      this.nextQuestion();
      this.$store.dispatch('quiz/GET_RESULT');
      this.thanksForSubmit = true;
    },

    setNewQuiz() {
      this.closeQuizPopup();
      this.$store.dispatch('quiz/FIRST_STEP');
    },

    closeQuizPopup() {
      this.$store.commit('popup/closeQuizPopup');
    },
  },
};
</script>

<style scoped>
.quiz__title {
  font-weight: 600;
  font-size: 32px;
  line-height: 1.12;
  margin: 0 0 40px;
}

.quiz__title_type_thanks {
  text-align: center;
  margin: 0 0 452px;
}

.quiz__question {
  font-size: 18px;
  line-height: 1.33;
  margin: 0;
  min-height: 72px;
  font-weight: 500;
}

.quiz__question_type_additional {
  font-weight: 400;
  color: #666;
}

.quiz__input {
  margin: 86px 0 0;
}

.quiz__submit {
  display: flex;
  margin-top: 210px;
  align-items: center;
}

.quiz__button_direction_back {
  background: none;
  color: #666;
  padding: 0;
  margin-right: 30px;
}

.quiz__button_direction_back:disabled {
  background: none;
  color: #c0c0c0;
  padding: 0;
  pointer-events: none;
}

.quiz__button_direction_next {
  font-weight: 500;
  width: 226px;
  padding: 16px 0;
  color: #fff;
}

.quiz__button_direction_next:disabled {
  background: #666;
}

.quiz__policy {
  font-size: 14px;
  line-height: 1.21;
  margin: 0 0 0 30px;
  max-width: 378px;
  color: #666;
}

.quiz__policy-link {
  cursor: pointer;
  color: #666;
  text-decoration: none;
  border-bottom: 1px solid #666;
}

.quiz__policy-link:hover {
  opacity: 0.8;
  transition: opacity 0.3s linear;
}

.quiz__thanks {
  display: flex;
  flex-direction: column;
  align-items: center;
}

@media screen and (max-width: 1280px) {
  .quiz__title {
    font-size: 28px;
    line-height: 1.14;
  }

  .quiz__title_type_thanks {
    margin: 0 0 412px;
  }

  .quiz__question {
    font-size: 16px;
    line-height: 1.37;
  }

  .quiz__submit {
    margin-top: 170px;
  }

  .quiz__button_direction_next {
    width: 200px;
  }
}

@media screen and (max-width: 1024px) {
  .quiz__title {
    font-size: 26px;
    line-height: 1.15;
  }

  .quiz__title_type_thanks {
    margin: 0 0 415px;
  }

  .quiz__question {
    font-size: 15px;
    line-height: 1.47;
  }

  .quiz__submit {
    margin-top: 174px;
  }
}

@media screen and (max-width: 768px) {
  .quiz__title_type_thanks {
    margin: 0 0 380px;
  }
  .quiz__question {
    min-height: 110px;
  }
  .quiz__input {
    margin: 30px 0 0;
  }

  .quiz__submit {
    flex-direction: column;
    align-items: flex-start;
  }

  .quiz__policy {
    margin: 40px 0 0;
  }
}
@media screen and (max-width: 475px) {
  .quiz__title {
    font-size: 18px;
    line-height: 1.17;
    margin: 0 0 30px;
  }

  .quiz__title_type_thanks {
    margin: 0 0 476px;
  }

  .quiz__question {
    font-size: 13px;
    line-height: 1.23;
    min-height: 90px;
  }

  .quiz__submit {
    margin-top: 250px;
  }

  .quiz__button {
    font-size: 13px;
    line-height: 1.23;
  }

  .quiz__button_direction_back {
    margin-right: 15px;
  }

  .quiz__button_direction_next {
    width: 180px;
    padding: 12px 0;
  }

  .quiz__policy {
    max-width: 350px;
    font-size: 11px;
    line-height: 1.18;
    margin-top: 15px;
  }
}
</style>
