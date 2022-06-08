<script lang="ts">
  export let title: string = localStorage.getItem('title') || 'Welcome to big timer';
  export let subtitle: string = localStorage.getItem('subtitle') || 'Customize your settings and then start the timer.';
  export let currentTime: string = '5:00';
  export let running: boolean = false;
  export let settingsHidden: boolean = true;

  let timer: any;
  let minutes: number = 5;
  let seconds: number = 0;
  let currentMinutes: number = 5;
  let currentSeconds: number = 0;

  $: localStorage.setItem('title', title);
  $: localStorage.setItem('subtitle', subtitle);
  $: localStorage.setItem('minutes', minutes.toString());
  $: localStorage.setItem('seconds', seconds.toString());

  const toggleTimer = () => {
    running = !running;
    console.log(running);
    if (running) {
      settingsHidden = true;

      // Check if we're restarting the countdown.
      if (currentMinutes === 0 && currentSeconds === 0) {
        currentMinutes = minutes;
        currentSeconds = seconds;
      }

      // Set the second timer.
      timer = setInterval(() => {
        if (currentSeconds > 0) {
          currentSeconds--;
        } else {
          if (currentMinutes > 0) {
            currentMinutes--;
            currentSeconds = 59;
          } else {
            clearInterval(timer);
            running = false;
            currentMinutes = 0;
            currentSeconds = 0;
          }
        }
        setCurrentTime();
      }, 1000);
    } else {
      clearInterval(timer);
    }
  };

  const resetTimer = () => {
      clearInterval(timer);
      running = false;
      currentMinutes = minutes;
      currentSeconds = seconds;
      setCurrentTime();
  }

  const setCurrentTime = () => {
      currentTime = `${currentMinutes}:${currentSeconds < 10 ? '0' : ''}${currentSeconds}`;
  }
</script>

<main>
  <h1>Settings</h1>
  <div class="options">
      <div class="option">
        <label for="title">Title</label>
        <input name="title" class="input" type="text" bind:value={title} />
      </div>
      <div class="option">
        <label for="subtitle">Subtitle</label>
        <input name="subtitle" class="input" type="text" bind:value={subtitle} />
      </div>
      <div class="option">
        <label for="countdownTime">Countdown Time (mm:ss)</label>
        <input name="countdownTime" class="time-input" type="number" min="0" on:keyup={() => resetTimer()} bind:value={minutes} />:
        <input name="countdownTime" class="time-input" type="number" min="0" on:keyup={() => resetTimer()} bind:value={seconds} />
      </div>
      <!-- <div class="option arrow">
          <input type=radio bind:group={theme} name="theme" value="light"><span on:click={() => theme = 'light'}>Light mode</span>
          <input type=radio bind:group={theme} name="theme" value="dark"><span on:click={() => theme = 'dark'}>Dark mode</span>
      </div> -->
  </div>
  <div class="options">
        <div class="option">
          <button on:click={() => toggleTimer()}>{ running ? 'Pause' : 'Start' }</button>
          <button on:click={() => resetTimer()}>Reset</button>
      </div>
  </div>
</main>
<hr />

<style lang="scss">
  main {
    text-align: center;
    padding: 0 1em 1em 1em;
    margin: 0 auto;
  }

  h1 {
    font-size: 4em;
    font-weight: 100;
    margin-top: 0;
    margin-bottom: .5rem;
  }

  .input {
    min-width: 400px;
  }

  .time-input {
      width: 3rem;
  }

  .options {
    display: flex;
    gap: 3rem;
    align-items: baseline;
    justify-content: center;
    margin-top: 1rem;
  }

  .option {
    display: flex;
    flex-direction: row;
    align-items: baseline;
    gap: 0.5rem;
  }

  label {
      font-weight: bold;
  }

//   .arrow {
//       cursor: pointer;
//   }

  button {
      min-width: 10rem;
  }
</style>
