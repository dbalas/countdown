<script lang="ts">
  import { onMount, onDestroy } from "svelte";
  import confetti from "canvas-confetti";

  const targetDate = new Date("2025-03-31T00:00:00");
  let days = 0;
  let hours = 0;
  let minutes = 0;
  let seconds = 0;
  let isTargetReached = false;
  let interval: number;

  function updateCountdown() {
    const now = new Date();
    const difference = targetDate.getTime() - now.getTime();

    if (difference > 0) {
      isTargetReached = true;
      clearInterval(interval);

      // Trigger confetti
      const duration = 100 * 1000;
      const animationEnd = Date.now() + duration;
      const defaults = {
        startVelocity: 100,
        spread: 360,
        ticks: 60,
        zIndex: 0,
      };

      function randomInRange(min: number, max: number) {
        return Math.random() * (max - min) + min;
      }

      interval = setInterval(function () {
        const timeLeft = animationEnd - Date.now();

        if (timeLeft <= 0) {
          return clearInterval(interval);
        }

        const particleCount = 50 * (timeLeft / duration);

        confetti({
          ...defaults,
          particleCount,
          origin: { x: randomInRange(0.1, 0.3), y: Math.random() - 0.2 },
        });
        confetti({
          ...defaults,
          particleCount,
          origin: { x: randomInRange(0.7, 0.5), y: Math.random() - 0.2 },
        });
        confetti({
          ...defaults,
          particleCount,
          origin: { x: randomInRange(0.7, 0.9), y: Math.random() - 0.2 },
        });
      }, 500);

      return;
    }

    days = Math.floor(difference / (1000 * 60 * 60 * 24));
    hours = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
    minutes = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
    seconds = Math.floor((difference % (1000 * 60)) / 1000);
  }

  onMount(() => {
    updateCountdown();
    interval = setInterval(updateCountdown, 1000);
  });

  onDestroy(() => {
    if (interval) clearInterval(interval);
  });
</script>

{#if isTargetReached}
  <div class="celebration">
    <h1>It's the day!</h1>
  </div>
{:else}
  <div class="countdown">
    <div class="gif-container">
      <img
        src="https://media.giphy.com/media/9SIXFu7bIUYHhFc19G/giphy.gif"
        alt="Anxious waiting gif"
        class="anxious-gif"
      />
    </div>
    <h2>Time until March 31, 2025</h2>
    <div class="timer">
      <div class="time-block">
        <span class="number">{days}</span>
        <span class="label">Days</span>
      </div>
      <div class="time-block">
        <span class="number">{hours}</span>
        <span class="label">Hours</span>
      </div>
      <div class="time-block">
        <span class="number">{minutes}</span>
        <span class="label">Minutes</span>
      </div>
      <div class="time-block">
        <span class="number">{seconds}</span>
        <span class="label">Seconds</span>
      </div>
    </div>
  </div>
{/if}

<style>
  .countdown {
    text-align: center;
    padding: 1rem;
    max-height: 100vh;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  h2 {
    margin: 1rem 0;
    color: #ff6b00;
    font-size: clamp(1.5rem, 4vw, 2.5rem);
  }

  .timer {
    display: flex;
    justify-content: center;
    gap: clamp(0.5rem, 2vw, 2rem);
    margin-bottom: 1rem;
    flex-wrap: wrap;
  }

  .time-block {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 0.5rem;
  }

  .number {
    font-size: clamp(2rem, 6vw, 4.5rem);
    font-weight: bold;
    color: #ff6b00;
    line-height: 1;
  }

  .label {
    font-size: clamp(0.8rem, 2vw, 1.5rem);
    color: #666;
    text-transform: uppercase;
  }

  .celebration {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    width: 100%;
    background: #fff;
  }

  @keyframes backgroundBlink {
    0% {
      background-color: white;
    }
    50% {
      background-color: black;
    }
    100% {
      background-color: white;
    }
  }

  .celebration h1 {
    font-size: clamp(2.5rem, 8vw, 5rem);
    color: #ff6b00;
    text-align: center;
    animation: pulse 2s infinite;
    text-transform: uppercase;
  }

  .gif-container {
    margin-bottom: 1rem;
    width: 100%;
    max-width: 300px;
  }

  .anxious-gif {
    width: 100%;
    height: auto;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }

  @keyframes pulse {
    0% {
      transform: scale(1);
    }
    50% {
      transform: scale(1.1);
    }
    100% {
      transform: scale(1);
    }
  }

  @media (max-width: 480px) {
    .countdown {
      padding: 0.5rem;
    }

    .timer {
      gap: 1rem;
    }
  }
</style>
