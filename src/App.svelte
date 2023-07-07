<script>
  import twitterLogo from "./assets/twitter.svg";
  import values from "./data.json";

  function getRandomEntry() {
    const randomIndex = Math.floor(Math.random() * values.length);
    return values[randomIndex];
  }

  $: randomEntry = getRandomEntry();
</script>

<main>
  <div
    id="content"
    class="{randomEntry.class} h-4/6 w-3/4 shadow-md shadow-cyan-800/100 p-10"
  >
    <div class="text-lg">"{randomEntry.qoute}" <br /><br /><br /> {randomEntry.book}</div>
    <br />
    <div />
  </div>
  <div class="flex">
    <a
      class="twitter-share-button"
      href="https://twitter.com/intent/tweet?text={randomEntry.qoute} %0A %0A {randomEntry.book} %0A %0A {randomEntry.hashtags}"
      data-size="large"
    >
      <img src={twitterLogo} class="logo" alt="Svelte Logo" /></a
    >
  </div>
</main>

<style lang="postcss">
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
  }

  .logo:hover {
    filter: drop-shadow(0 0 1em #01aaec);
  }

  .deep-work {
    color: #000000;
    background-color: #f5d83c;
    box-shadow: 10px 10px 5px #000000;
  }
</style>
