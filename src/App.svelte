<script>
  import { onMount } from 'svelte';
  import Game from './lib/Game.svelte';
  import 'bootstrap/dist/css/bootstrap.min.css';
</script>

<main class="container mt-5">
  <h1 class="text-center mb-4">Emoji Guessing Game</h1>
  <Game />
</main>

<style>
  :global(body) {
    background-color: #f8f9fa;
  }
</style>