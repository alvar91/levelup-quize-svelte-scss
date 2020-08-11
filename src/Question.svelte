<script>
  import { score } from "./store.js";

  export let question;
  export let nextQuestion;

  let isCorrect;
  let isAnswered = false;
  let answers = question.incorrect_answers.map(answer => {
    return {
      answer,
      correct: false
    };
  });
  let allAnswers = [
    ...answers,
    {
      answer: question.correct_answer,
      correct: true
    }
  ];
  shuffle(allAnswers);

  function shuffle(array) {
    array.sort(() => Math.random() - 0.5);
  }

  function checkQuestion(correct) {
    if (!isAnswered) {
      isAnswered = true;
      isCorrect = correct;
      if (correct) {
        score.update(val => val + 1);
      }
    }
  }
</script>

<style>
  h5 {
    color: blueviolet;
  }

  h5.wrong {
    color: red;
  }

  h5.isCorrect {
    color: green;
  }

  .answer {
    display: block;
  }
</style>

<h3>
  {@html question.question}
</h3>
{#if isAnswered}
  <h5 class:isCorrect class:wrong={!isCorrect}>
    {#if isCorrect}You got it right{:else}You goofed up{/if}
  </h5>
{/if}

{#each allAnswers as answer}
  <button
    class="answer"
    disabled={isAnswered}
    on:click={() => checkQuestion(answer.correct)}>
    {@html answer.answer}
  </button>
{/each}

{#if isAnswered}
  <div>
    <button on:click={nextQuestion}>Next Question</button>
  </div>
{/if}
