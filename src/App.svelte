<script>
  // @ts-nocheck
  import twitterLogo from "./assets/twitter.svg";
  import values from "./data.json";
  import { onMount } from "svelte";
  import auth from "./authService";
  // @ts-ignore
  import { isAuthenticated, user, user_tasks, tasks } from "./store";

  function getRandomEntry() {
    const randomIndex = Math.floor(Math.random() * values.length);
    return values[randomIndex];
  }

  $: randomEntry = getRandomEntry();

  let auth0Client;
  let newTask;

  onMount(async () => {
    auth0Client = await auth.createClient();

    isAuthenticated.set(await auth0Client.isAuthenticated());
    user.set(await auth0Client.getUser());
  });

  // @ts-ignore
  function login() {
    auth.loginWithPopup(auth0Client);
  }

  // @ts-ignore
  function logout() {
    auth.logout(auth0Client);
  }

  // @ts-ignore
  function addItem() {
    let newTaskObject = {
      id: genRandom(),
      description: newTask,
      completed: false,
      // @ts-ignore
      user: $user.email,
    };

    console.log(newTaskObject);

    let updatedTasks = [...$tasks, newTaskObject];

    tasks.set(updatedTasks);

    newTask = "";
  }

  function genRandom(length = 7) {
    var chars =
      "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    var result = "";
    for (var i = length; i > 0; --i)
      result += chars[Math.round(Math.random() * (chars.length - 1))];
    return result;
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
