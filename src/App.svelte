<script>
  import twitterLogo from "./assets/twitter_x.svg";
  import googleLogo from "./assets/google-color.svg"
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
  <div class="w-full bg-amber-500 text-xl font-bold"> ⚠️ We are moving to <a class="text-white hover:text-black hover:underline" href="http://qoutesapp.com/?ref=qoutesv1">Qoutesapp.com</a></div>
  <div
    id="content"
    class="{randomEntry.class} h-4/6 lg:w-full md:w-full sm:w-full shadow-md p-10"
  >
  
    <div class="text-xl font-bold">
      {randomEntry.body} <br /><br /><br />
      <p class="font-medium">{randomEntry.title}</p>
    </div>
    <br />
    <div />
  </div>
  <div class="flex">
    
    <div class="pt-10">
      <!-- {#if $isAuthenticated}
        <a
          class="twitter-share-button disable"
          href="/"
          data-size="large"
          on:click={() => false}
        >
          <img
            src={twitterLogo}
            class="disabledlogo grayscale"
            alt="Svelte Logo"
          /></a
        >
        <button
          type="button"
          class="flex text-white font-black bg-gray-100 hover:bg-gray-800 focus:ring-4 focus:ring-gray-300 
          font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-gray-600 dark:hover:bg-gray-700 
          focus:outline-none dark:focus:ring-gray-800"
          on:click={login}>
          <img
            src={googleLogo}
            class="h-5 w-6 pr-2"
            alt="Google Logo"
          />
          Login via Google to Tweet</button
        >
      {:else}
        <a
          class="twitter-share-button disable"
          href="https://twitter.com/intent/tweet?text={randomEntry.qoute} %0A %0A {randomEntry.book} %0A %0A {randomEntry.hashtags}"
          data-size="large"
        >
          <img src={twitterLogo} class="logo" alt="Svelte Logo" /></a
        >
        <button
          type="button"
          class="text-gray-900 bg-white font-black border border-gray-300 focus:outline-none hover:bg-gray-100 focus:ring-4 focus:ring-gray-200 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-gray-800 dark:text-white dark:border-gray-600 dark:hover:bg-gray-700 dark:hover:border-gray-600 dark:focus:ring-gray-700"
          on:click={logout}>Log Out</button
        > 
      {/if} 
    -->
  </div>
</main>

<style lang="postcss">
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
     filter: drop-shadow(0 0 1em #ffffff);
    
  }

  .logo:hover {
    filter: drop-shadow(0 0 1em #01aaec);
  }
  .disabledlogo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
    transition: filter 300ms;
  }
</style>
