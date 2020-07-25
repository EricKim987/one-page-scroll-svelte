<script>
  // components
  import Indicator from "./Indicator.svelte";
  // store
  import { currentPage } from "./store/store.js";
  // svelte
  import { each, onMount } from "svelte/internal";
  import { quadInOut } from "svelte/easing";

  const pages = [0, 1, 2, 3, 4];
  let calling = false;
  let windowHeight = 0;
  // should match transition-duration css property
  const duration = 800;
  $: y = windowHeight * $currentPage;

  onMount(() => {
    windowHeight = window.innerHeight;
    window.onresize = resizeHandler;
  });
  function resizeHandler(e) {
    windowHeight = window.innerHeight;
  }

  function throttle(func, limit) {
    if (!calling) {
      func();
      calling = true;
      setTimeout(() => (calling = false), limit);
    }
  }
  function movePage(e) {
    const scroll = () => {
      const { deltaY } = e;
      if (deltaY > 0 && $currentPage < pages.length - 1) {
        currentPage.update((value) => value + 1);
      } else if (deltaY < 0 && $currentPage > 0) {
        currentPage.update((value) => value - 1);
      } else {
        return false;
      }
    };
    throttle(scroll, duration);
  }
</script>

<style>
  :global(body) {
    padding: 0;
  }
  main {
    overflow: hidden;
    height: 100vh;
  }
  .container {
    transition: all 0.8s ease-in-out;
  }
  .page {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100vh;
  }
</style>

<main on:wheel={movePage}>
  <Indicator length={pages.length} />
  <div class="container" style="transform: translate3d(0px, {-y}px, 0px)">
    {#each pages as page}
      <section class="page" style="background:hsl({40 + page * 40},70%,50%)">
        {`page${page}`}
      </section>
    {/each}
  </div>
</main>
