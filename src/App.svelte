<script>
  import twitterLogo from "./assets/twitter.svg";
  import values from "./data.json";

  function getRandomEntry() {
    const randomIndex = Math.floor(Math.random() * values.length);
    return values[randomIndex];
  }

  $: randomEntry = getRandomEntry();

  import { onMount } from "svelte";
  import auth from "./authService";
  // @ts-ignore
  import { isAuthenticated, user, user_tasks, tasks } from "./store";
  // @ts-ignore
  import TaskItem from "./components/TaskItem.svelte";

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
  </div>
  <!-- App Bar -->
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <a class="navbar-brand" href="/#">Task Manager</a>
    <button
      class="navbar-toggler"
      type="button"
      data-toggle="collapse"
      data-target="#navbarText"
      aria-controls="navbarText"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon" />
    </button>
    <div class="collapse navbar-collapse" id="navbarText">
      <div class="navbar-nav mr-auto user-details">
        {#if $isAuthenticated}
          <span class="text-white"
            >&nbsp;&nbsp;{$user.name} ({$user.email})</span
          >
        {:else}<span>&nbsp;</span>{/if}
      </div>
      <span class="navbar-text">
        <ul class="navbar-nav float-right">
          {#if $isAuthenticated}
            <li class="nav-item">
              <a class="nav-link" href="/#" on:click={logout}>Log Out</a>
            </li>
          {:else}
            <li class="nav-item">
              <a class="nav-link" href="/#" on:click={login}>Log In</a>
            </li>
          {/if}
        </ul>
      </span>
    </div>
  </nav>

  <!-- Application -->
  {#if !$isAuthenticated}
    <div class="container mt-5">
      <div class="row">
        <div class="col-md-10 offset-md-1">
          <div class="jumbotron">
            <h1 class="display-4">Task Management made Easy!</h1>
            <p class="lead">Instructions</p>
            <ul>
              <li>Login to start &#128272;</li>
              <li>Create Tasks &#128221;</li>
              <li>Tick off completed tasks &#9989;</li>
            </ul>
            <a
              class="btn btn-primary btn-lg mr-auto ml-auto"
              href="/#"
              role="button"
              on:click={login}>Log In</a
            >
          </div>
        </div>
      </div>
    </div>
  {:else}
    <div class="container" id="main-application">
      <div class="row">
        <div class="col-md-6">
          <ul class="list-group">
            {#each $user_tasks as item (item.id)}
              <TaskItem task={item} />
            {/each}
          </ul>
        </div>
        <div class="col-md-6">
          <input
            class="form-control"
            bind:value={newTask}
            placeholder="Enter New Task"
          />
          <br />
          <button type="button" class="btn btn-primary" on:click={addItem}>
            Add Task
          </button>
        </div>
      </div>
    </div>
  {/if}
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
