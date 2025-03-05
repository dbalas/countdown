<script lang="ts">
  import Countdown from './lib/Countdown.svelte';
</script>

<main>
  <Countdown />
</main>

<style>
  main {
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;
  }

  :global(body) {
    margin: 0;
    padding: 0;
    overflow: hidden;
  }
</style>