<script>
  import * as d3 from "d3";
  import Scroller from "@sveltejs/svelte-scroller";
  import DataPoints from "./DataPoints.svelte";
  import { onMount } from "svelte";

  let data = [];

 onMount(async () => {
            const res = await fetch('iris.csv'); 
            const csv = await res.text();
            data = d3.csvParse(csv, d3.autoType)
            console.log(data);
        });
</script>

<main>
  <h1>Svelte template</h1>
  <DataPoints {data} />
  <p>Write your HTML here</p>
</main>

<style>
</style>
