<script>
  import twitterLogo from "./assets/twitter.svg";

  let values = [
    {
      qoute:
        "\"searing memoir of reckoning and healing by acclaimed journalist Stephanie Foo, investigating the little-understood science behind compl\"",
      "primary-color": "#edB989",
      "contrast-color": "edB989",
      book: "What My Bones Know: A Memoir of Healing from Complex Trauma",
      image:
        "https://i.gr-assets.com/images/S/compressed.photo.goodreads.com/books/1635810826i/59515912._SY180_.jpg",
    },
  ];
</script>

<main>
  <div class="flex">
    <a
      class="twitter-share-button"
      href="https://twitter.com/intent/tweet?text={values[0].qoute}"
      data-size="large"
    >
      <img src={twitterLogo} class="logo" alt="Svelte Logo" /></a
    >
  </div>

  <div class={`h-4/6 w-screen shadow-md shadow-cyan-800/100 p-10 bg-gray-800`}>
    <div class="read-the-docs">{values[0].qoute}</div>
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
    filter: drop-shadow(0 0 1em #01AAEC);
  }



  .read-the-docs {
    color: #888;
  }
</style>
