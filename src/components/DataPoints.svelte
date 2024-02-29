<script>
    import * as d3 from "d3";
    export let data

    const width = 928;
    const height = 500;
    const marginTop = 20;
    const marginRight = 30;
    const marginBottom = 20;
    const marginLeft = 40;

    let svg;
    let gx;
    let gy;
    let k;
    let data_class = {};

    $: x = d3
    .scaleLinear()
    .domain([0, d3.max(data, (d) => d.petal_width)])
    .range([marginLeft, width - marginRight])

    $: y = d3
    .scaleLinear()
    .domain([0, d3.max(data, (d) => d.petal_length)])
    .range([height - marginBottom, marginTop])

    $: d3.select(gx).call(d3.axisBottom(x)) 

    $: d3.select(gy).call(d3.axisLeft(y))

    $: max = d3.max(data, (d) => Math.abs(d.sepal_length))
    $: min = d3.min(data, (d) => d.sepal_length)

    $: color = d3
      .scaleSequential()
      .domain([min, max])
      .interpolator(d3.interpolateViridis)

    $: console.log(data);

    $: circleSize = d3
      .scaleLinear()
      .domain([d3.min(data,(d) => d.sepal_width), d3.max(data, (d) => d.sepal_width)])
      .range([0, 15])

    $: rectangleSize = d3
      .scaleLinear()
      .domain([d3.min(data,(d) => d.sepal_width), d3.max(data, (d) => d.sepal_width)])
      .range([10, 30])

    $: triangleSize = d3
      .scaleLinear()
      .domain([d3.min(data,(d) => d.sepal_width), d3.max(data, (d) => d.sepal_width)])
      .range([30, 60])

      function classification(data, k) {
        let result = {}
        let distance;
        for (let i = 0; i < data.length; i++) {
          let distance_dict = {};
          let classes = {'Iris-virginica': 0, 'Iris-setosa': 0, "Iris-versicolor": 0};
          for (let j = 0; j < data.length; j++) {
            if (i === j) {
              continue;
            } else {
              distance = Math.sqrt(((data[i].petal_length - data[j].petal_length) ** 2) + ((data[i].petal_width - data[j].petal_width) ** 2) + ((data[i].sepal_length - data[j].sepal_length) ** 2) + ((data[i].sepal_width - data[j].sepal_width) ** 2))
              distance_dict[j] = distance;
            }
          }
          var dist_items = Object.keys(distance_dict).map(function(key) {
            return [key, distance_dict[key]];
          });
          dist_items.sort(function(first, second) {
            return first[1] - second[1];
          });
          for (let n = 0; n < k; n++){
            classes[data[dist_items[n][0]].class] = classes[data[dist_items[n][0]].class] + 1;
          }
          var class_items = Object.keys(classes).map(function(key) {
            return [key, classes[key]];
          });
          class_items.sort(function(first, second) {
            return second[1] - first[1];
          });
          result[i] = class_items[0][0]
          }
          return result;
      }

      $: console.log(classification(data, 3));
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
      <!-- $(document).ready(function(){
        console.log(circleSize(d.sepal_width))
      }); -->
        {#if d.class === "Iris-setosa"}
          <circle key = {i} cx = {x(d.petal_width)} cy = {y(d.petal_length)} fill = {color(d.sepal_length)} stroke = "#000" r = {circleSize(d.sepal_width)}/>
        {/if}
        {#if d.class === "Iris-versicolor"}
          <rect key = {i} width = {rectangleSize(d.sepal_width)} height = {rectangleSize(d.sepal_width)} x = {x(d.petal_width)} y = {y(d.petal_length)} fill = {color(d.sepal_length)} stroke = "#000"/>
        {/if}
        {#if d.class === "Iris-virginica"}
          <!-- <circle key = {i} cx = {x(d.petal_width)} cy = {y(d.petal_length)} fill = {color(d.sepal_length)} stroke = "#000" r = "5"/> -->
          <!-- <polygon key = {i} points = "{x(d.petal_width)}, 10, 10, 10, 10, {y(d.petal_length)}" fill = {color(d.sepal_length)} stroke = "#000"/> -->
          <g transform = {`translate(${x(d.petal_width)}, ${y(d.petal_length)})`}>
            <path d="M{triangleSize(d.sepal_width)/4},{triangleSize(d.sepal_width)/2} h{triangleSize(d.sepal_width)/2} l{-triangleSize(d.sepal_width)/4},{-triangleSize(d.sepal_width)/2} Z" fill = {color(d.sepal_length)} stroke = "#000"/>
          </g>
        {/if}
      {/each}
    {/if}
    <text x = 420 
    y = 530 
    font-size = 20>Petal Width</text>
    <text 
    font-size = 20
    x = 10
    y = -300
    style = "writing-mode: vertical-lr; transform: rotate(180deg);">Pedal Length</text>
  </svg>
</div>

<style>
  circle {
    fill-opacity: 100%;
    stroke-opacity: 100%;
  }
  rect {
    fill-opacity: 100%;
    stroke-opacity: 100%;
  }
</style>