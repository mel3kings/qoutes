<script>
  import twitterLogo from "./assets/twitter.svg";
  import values from "./data.json";
  import { onMount } from "svelte";
  import auth from "./authService";
  import { isAuthenticated, user, tasks } from "./store";

  let auth0Client;

  function getRandomEntry() {
    const randomIndex = Math.floor(Math.random() * values.length);
    return values[randomIndex];
  }
  $: randomEntry = getRandomEntry();

  onMount(async () => {
    auth0Client = await auth.createClient();

    isAuthenticated.set(await auth0Client.isAuthenticated());
    user.set(await auth0Client.getUser());
  });

  function login() {
    auth.loginWithPopup(auth0Client);
  }

  function logout() {
    auth.logout(auth0Client);
  }
</script>

<main>
  <div
    id="content"
    class="{randomEntry.class} h-4/6 lg:w-3/4 md:w-full sm:w-full shadow-md p-10"
  >
    <div class="text-xl font-bold">
      "{randomEntry.qoute}" <br /><br /><br />
      <p class="font-medium">{randomEntry.book}</p>
    </div>
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
    <div class="pt-10">
      {#if !$isAuthenticated}
        <a
          class="btn btn-primary btn-lg mr-auto ml-auto"
          href="/#"
          role="button"
          on:click={login}>Log In</a
        >
      {:else}
        <a class="nav-link" href="/#" on:click={logout}>Log Out</a>
      {/if}
    </div>
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
</style>
