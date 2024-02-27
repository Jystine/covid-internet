<script>
  import * as d3 from "d3";
  import Scroller from "@sveltejs/svelte-scroller";

  let count, index, offset, progress;

</script>

<main>
  <h1>Svelte template</h1>

  <p>Write your HTML here</p>
</main>

<Scroller
  top = {0.0}
  bottom = {1}
  threshold = {0.5}
  bind:count
  bind:index
  bind:offset
  bind:progress>

  <div class = "foreground" slot = "foreground">
    <section>This is the first section.</section>
    <section>This is the second section.</section>
    <section>This is the third section.</section>
    <section>This is the fourth section.</section>
    <section>This is the fifth section.</section>
    <section>This is the sixth section.</section>
  </div>

</Scroller>

<style>
  section {
    height: 80vh;
    background-color: rgba(0, 0, 0, 0.2); /* 20% opaque */
    /* color: white; */
    outline: magenta solid 3px;
    text-align: center;
    max-width: 4474px; /* adjust at will */
    color: black;
    padding: 1em;
    margin: 2em 2em 2em 2em;
  }
</style>
