<script>
  import { onMount } from 'svelte';
  import questions from '../data/questions.js';

  let categories = Object.keys(questions);
  let selectedCategory = categories[0];
  let currentQuestion = 0;
  let score = 0;
  let gameOver = false;
  let timer = 15;
  let revealedEmojis = 1;
  let intervalId;
  let gameStarted = false;

  function startGame() {
    currentQuestion = 0;
    score = 0;
    gameOver = false;
    gameStarted = true;
    nextQuestion();
  }

  function nextQuestion() {
    if (currentQuestion < questions[selectedCategory].length) {
      timer = 15;
      revealedEmojis = 1;
      startTimer();
    } else {
      gameOver = true;
    }
  }

  function startTimer() {
    clearInterval(intervalId);
    intervalId = setInterval(() => {
      if (timer > 0) {
        timer--;
        if (timer % 3 === 0 && revealedEmojis < 5) {
          revealedEmojis++;
        }
      } else {
        clearInterval(intervalId);
        nextQuestion();
      }
    }, 1000);
  }

  function checkAnswer(answer) {
    clearInterval(intervalId);
    if (answer === questions[selectedCategory][currentQuestion].correct) {
      score++;
    }
    currentQuestion++;
    nextQuestion();
  }

  onMount(() => {
    // Game will start when the user clicks the "Start Game" button
  });
</script>

<div class="game-container">
  {#if !gameStarted}
    <div class="start-screen">
      <h4>Select a Category</h4>
      <select bind:value={selectedCategory} class="form-select mb-3">
        {#each categories as category}
          <option value={category}>{category}</option>
        {/each}
      </select>
      <button class="btn btn-primary" on:click={startGame}>Start Game</button>
    </div>
  {:else if !gameOver}
    <div class="question-container">
      <h2>{selectedCategory} - Question {currentQuestion + 1}</h2>
      <div class="emojis">
        {#each questions[selectedCategory][currentQuestion].emojis.slice(0, revealedEmojis) as emoji}
          <span class="emoji">{emoji}</span>
        {/each}
      </div>
      <div class="timer">Time left: {timer}s</div>
      <div class="options">
        {#each questions[selectedCategory][currentQuestion].options as option}
          <button class="btn btn-primary m-2" on:click={() => checkAnswer(option)}>{option}</button>
        {/each}
      </div>
    </div>
  {:else}
    <div class="game-over">
      <h2>Game Over!</h2>
      <p>Your score: {score} out of {questions[selectedCategory].length}</p>
      <button class="btn btn-success" on:click={() => { gameStarted = false; }}>Play Again</button>
    </div>
  {/if}
</div>

<hr style="margin-top: 50px">

<div class="video-container">
  <h3>How this game was built</h3>
  <p>Watch the video below to see a walkthrough of how this Emoji Guessing Game was created using <a href="https://bolt.new">bolt.new</a>:</p>
  <iframe
    width="560"
    height="315"
    src="https://www.youtube.com/embed/cSDWr2WdpqI"
    title="YouTube video player"
    frameborder="0"
    allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
    allowfullscreen
  ></iframe>
</div>

<style>
  .game-container {
    max-width: 600px;
    margin: 0 auto;
    text-align: center;
  }
  .emojis {
    font-size: 2rem;
    margin-bottom: 1rem;
  }
  .emoji {
    margin: 0 0.2rem;
  }
  .timer {
    font-size: 1.2rem;
    margin-bottom: 1rem;
  }
  .options {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  .start-screen {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .video-container {
    max-width: 600px;
    margin: 2rem auto;
    text-align: center;
  }
  .video-container h3 {
    margin-bottom: 0.5rem;
  }
  .video-container p {
    margin-bottom: 1rem;
  }
  .video-container iframe {
    max-width: 100%;
  }
</style>