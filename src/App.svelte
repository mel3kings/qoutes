<script>
  import twitterLogo from "./assets/twitter.svg";

  let values = [
    {
      qoute:
        '"searing memoir of reckoning and healing by acclaimed journalist Stephanie Foo, investigating the little-understood science behind compl"',
      book: "What My Bones Know: A Memoir of Healing from Complex Trauma",
      class: "deep-work",
      hashtags: "%23goodreads %23qoutes",
      image:
        "https://images-na.ssl-images-amazon.com/images/S/compressed.photo.goodreads.com/books/1447957962i/25744928.jpg",
    },
  ];
</script>

<main>
  <div
    id="content"
    class="{values[0].class} h-4/6 w-3/4 shadow-md shadow-cyan-800/100 p-10"
  >
    <div>{values[0].qoute} | {values[0].book}</div>
    <br />
    <div />
  </div>
  <div class="flex">
    <a
      class="twitter-share-button"
      href="https://twitter.com/intent/tweet?text={values[0]
        .qoute} %0A %0A {values[0].book} %0A %0A {values[0].hashtags}"
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
