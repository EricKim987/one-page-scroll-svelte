<script>
  import { currentPage } from "./store/store.js";
  import { each } from "svelte/internal";

  export let length;

  function changePage(pageNumber) {
    currentPage.set(pageNumber);
  }
</script>

<style>
  .container {
    position: fixed;
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 10px;
    right: 20px;
    top: 50%;
    transform: translate3d(0, -50%, 0);
    z-index: 10;
  }
  .dot {
    background: #fff;
    width: 5px;
    height: 5px;
    border-radius: 50%;
    margin: 5px 0px;
    transition: all 0.8s cubic-bezier(0.25, 1, 0.5, 1);
  }
  .dot:hover {
    cursor: pointer;
  }
  .dot-active {
    background: fuchsia;
    width: 10px;
    height: 10px;
    margin: 10px 0px;
  }
</style>

<div class="container">
  {#each Array(length) as _, index}
    <div
      class={index === $currentPage ? 'dot-active dot' : 'dot'}
      on:click={() => changePage(index)} />
  {/each}
</div>
