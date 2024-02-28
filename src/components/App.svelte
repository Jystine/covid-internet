<script>
  import * as d3 from "d3";
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
  <h1>KNN Machine Learning</h1>
  <p style = "padding:20px;">Introduction to KNN</p>
  <DataPoints {data}/>
  <p style = "padding:20px;">Write your HTML here</p>
</main>

<style>
  main {
    text-align: center;
  }
</style>
