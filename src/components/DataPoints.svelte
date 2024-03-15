<script>
  import * as d3 from "d3";
  export let data

  const width = 928;
  const height = 500;
  const marginTop = 20;
  const marginRight = 30;
  const marginBottom = 20;
  const marginLeft = 40;

  let k_input;
  let k = 3;
  let path = "k_3.csv";
  let button1;
  let button2;
  let valid = true;
  $: button_label = `k = ${k}`;

  
  let svg;
  let gx;
  let gy;
  let show_true = false;
  $: button_t = button_text(show_true);
  
  $: accuracy_score = accuracy(data);

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

  function accuracy(data) {
    let correct = 0;
    if (data.length !== 0) {
      for (let i = 2750; i >= 2601; i = i - 1) {
        if (data[i].class === data[i].predicted) {
          correct = correct + 1;
        }
      }
      return Math.round((correct / 150).toFixed(2) * 100);
    }
  }

  function button_text(status) {
    if(status === false) {
      return "Show Actual Data"
    } else if (status === true) {
      return "Hide Actual Data"
    }
  }

  async function update(path) {
      console.log(path);
      const res = await fetch(path);
      const csv = await res.text();
      data = d3.csvParse(csv, d3.autoType)
    }

    function path_construction(input, path) {
      if (input >= 1 && input <= 150 && Number.isInteger(Number(input))) {
        let string = String(Number(input));
        let first_part  = "k_"
        let last_part = ".csv"
        let result = first_part.concat(string, last_part)
        return result;
      } else {
        return path
      }
    }

    function determine_k(k_input, k) {
      if (k_input === undefined) {
        return 3;
      } else if (k_input >= 1 && k_input <= 150 && Number.isInteger(Number(k_input))) {
        return String(Number(k_input));
      } else {
        return k;
      }
    }

    function determine_valid(k_input) {
      if (k_input !== undefined) {
        if (k_input >= 1 && k_input <= 150 && Number.isInteger(Number(k_input))) {
          return true
        } else {
          return false
        }
      }
    }

  $: console.log(typeof k_input);
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
      {#if d.type === "boundary"}
        {#if d.predicted === "Iris-setosa"}
          <rect key = {i} width = 20 height = 15 x = {x(d.petal_width)} y = {y(d.petal_length) - 10} fill = "#4059AD" r = "5"/>
        {/if}
        {#if d.predicted === "Iris-versicolor"}
          <rect key = {i} width = 20 height = 15 x = {x(d.petal_width)} y = {y(d.petal_length) - 10} fill = "#97D8C4" r = "5"/>
        {/if}
        {#if d.predicted === "Iris-virginica"}
          <rect key = {i} width = 20 height = 15 x = {x(d.petal_width)} y = {y(d.petal_length) - 10} fill = "#F4B942" r = "5"/>
        {/if}
      {/if}
      {#if show_true === false && d.type === "point"}
        {#if d.predicted === "Iris-setosa"}
          <circle key = {i} cx = {x(d.petal_width)} cy = {y(d.petal_length)} fill = "#4059AD" stroke = "#000" r = "5"/>
        {/if}
        {#if d.predicted === "Iris-versicolor"}
          <circle key = {i} cx = {x(d.petal_width)} cy = {y(d.petal_length)} fill = "#97D8C4" stroke = "#000" r = "5"/>
        {/if}
        {#if d.predicted === "Iris-virginica"}
          <circle key = {i} cx = {x(d.petal_width)} cy = {y(d.petal_length)} fill = "#F4B942" stroke = "#000" r = "5"/>
        {/if}
      {/if}
      {#if show_true === true && d.type === "point"}
        {#if d.class === "Iris-setosa"}
          <circle key = {i} cx = {x(d.petal_width)} cy = {y(d.petal_length)} fill = "#4059AD" stroke = "#000" r = "5"/>
        {/if}
        {#if d.class === "Iris-versicolor"}
          <circle key = {i} cx = {x(d.petal_width)} cy = {y(d.petal_length)} fill = "#97D8C4" stroke = "#000" r = "5"/>
        {/if}
        {#if d.class === "Iris-virginica"}
          <circle key = {i} cx = {x(d.petal_width)} cy = {y(d.petal_length)} fill = "#F4B942" stroke = "#000" r = "5"/>
        {/if}
      {/if}
    {/each}
  {/if}

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

  <g class = "accuracy_score">
    <text>Accuracy Score: {accuracy_score}%</text>
  </g>

  </svg>
</div>



  <div class = "label" style = "transform: translate(-16.5%, 150%)">
    <label id = "label">{button_label}</label>
  </div>
  <div id = "input-box" class = "w-full md:w-1/4 px-3" style = "transform: translate(80%, 120%)">
    <input class = "shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value = {k_input} type = "text" id = "k" placeholder = "Insert values from 1-150" />
    <!-- {#if valid === true}
    <input class = "shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" bind:value = {k_input} type = "text" id = "k" placeholder = "Insert values from 1-150" />
    {/if}
    {#if valid === false}
    <input type="text" id="error" bind:value = {k_input} 
    class="bg-red-50 border border-red-500 text-red-900 placeholder-red-700 text-sm rounded-lg focus:ring-red-500 dark:bg-gray-700 focus:border-red-500 block w-full p-2.5 dark:text-red-500 dark:placeholder-red-500 dark:border-red-500"
    placeholder="Error input">
    {/if} -->
  </div>
  <div id = "classify_button" style = "transform: translate(-16.5%, 140%)">
      <button bind:this={button2} class = "
        bg-white 
        hover:bg-gray-100 
         text-gray-800 
        font-semibold py-2 px-4 border 
        border-gray-400 rounded shadow
        "
        on:click={() => {k = determine_k(k_input, k), path = path_construction(k_input, path), valid = determine_valid(k_input), update(path)}}>Classify</button>
    </div>
   <div style = "transform: translate(-16.5%, 100%)">
    <button bind:this = {button1} on:click = {() => {show_true = !show_true}} style = "margin: 10px" 
      class = "
      bg-white 
      hover:bg-gray-100 
      text-gray-800 
      font-semibold py-2 px-4 border 
      border-gray-400 rounded shadow"
      >
      {button_t}
    </button>
  </div>
  <div id = "valid">
    {#if valid === false}
      <p class = "text-red-500 text-l italic container mx-auto w-24" style = "transform: translate(0, -350%)">Not a valid k</p>
    {/if}
  </div>

<!-- <style>
  rect {
    position: relative;
    z-index: 6;
  }
  circle {
    position: relative;
    z-index: 3;
    filter: brightness(75%);
  }
  button {
    margin-bottom: 1em;
    transform: translate(0, 150%)
  }
  button:hover {
    cursor: pointer;
  }
  .overlay {
    transform: translate(0, 10%);
  }
  .textbox {
    transform: translate(0, 150%);
  }
  .input {
    transform: translate(0, 15%);
  }
  .label{
    transform: translate(0, 150%);
  }
  .valid {
    transform: translate(0, 150%);
    color: red;
    font-weight: bold;
  }
</style> -->