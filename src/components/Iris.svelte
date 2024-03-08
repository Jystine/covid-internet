<script>
  import * as d3 from "d3";
  export let iris_data

  const width = 928;
  const height = 500;
  const marginTop = 20;
  const marginRight = 30;
  const marginBottom = 20;
  const marginLeft = 40;

  let svg;
  let gx;
  let gy;

  $: x = d3
    .scaleLinear()
    .domain([0, d3.max(iris_data, (d) => d.petal_width)])
    .range([marginLeft, width - marginRight])

  $: y = d3
    .scaleLinear()
    .domain([0, d3.max(iris_data, (d) => d.petal_length)])
    .range([height - marginBottom, marginTop])

  $: d3.select(gx).call(d3.axisBottom(x))
  $: d3.select(gy).call(d3.axisLeft(y))
  $: console.log(iris_data)
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

  <text x = 420 
    y = 530 
    font-size = 20
    style = "text-align:center">Petal Width</text>
  <text 
    font-size = 20
    x = 10
    y = -300
    style = "writing-mode: vertical-lr; transform: rotate(180deg);">Pedal Length</text>
  <g class = "legend" stroke = "#000">
    <text x = 1030 y = 58 font-size = 20>Legend</text>
    <circle cx = 1000 cy = 100 fill = "#4059AD" stroke = "#000" r = "13"/>
    <text x = 1030 y = 108 font-size = 20>Iris-setosa</text>
    <circle cx = 1000 cy = 150 fill = "#97D8C4" stroke = "#000" r = "13"/>
    <text x = 1030 y = 158 font-size = 20>Iris-versicolor</text>
    <circle cx = 1000 cy = 200 fill = "#F4B942" stroke = "#000" r = "13"/>
    <text x = 1030 y = 208 font-size = 20>Iris-virginica</text>
  </g>

  </svg>
</div>