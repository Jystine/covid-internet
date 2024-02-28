<script>
    import * as d3 from "d3";
    export let data

    const width = 928;
    const height = 500;
    const marginTop = 20;
    const marginRight = 30;
    const marginBottom = 30;
    const marginLeft = 40;

    let svg;
    let gx;
    let gy;

    $: x = d3
    .scaleLinear()
    .domain(d3.extent(data, (d) => d.petal_width))
    .range([marginLeft, width - marginRight])

    $: y = d3
    .scaleLinear()
    .domain(d3.extent(data, (d) => d.petal_length))
    .range([height - marginBottom, marginTop])

    $: d3.select(gx).call(d3.axisBottom(x)) //Makes gx a d3 object and pass in x scale
    $: d3.select(gy).call(d3.axisLeft(y))

    $: console.log(data);

</script>

<div class = "plot">
    <svg
      bind:this = {svg}
      {width}
      {height}
      viewBox = "0 0 {width} {height}"
      style = "max-width: 100%; height: auto;"
      overflow = "visible"
    >

    <g bind:this = {gx} transform = "translate(0, {height - marginBottom})"/>
    <g bind:this = {gy} transform = "translate({marginLeft}, 0)"/>

    {#if data.length !== 0}
      {#each data as d, i}
      $(document).ready(function() {
        console.log(y(d.petal_width))
      });
        <circle key = {i} cx = {x(d.petal_width)} cy = {y(d.petal_length)} r = "2.5"/>
      {/each}
    {/if}
  </svg>
</div>

<style>

</style>