<script>
  import * as d3 from "d3";
  export let data

  const width = 868;
  const height = 440;
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

<div id = "plot" class = "absolute">
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

  <text x = 385 
    y = 470 
    font-size = 20
    style = "text-align:center">Petal Width</text>
    <text 
    font-size = 20
    x = 10
    y = -280
    style = "writing-mode: vertical-lr; transform: rotate(180deg);">Pedal Length</text>

    <g class = "legend" stroke = "#000">
      <circle cx = 920 cy = 100 fill = "#4059AD" stroke = "#000" r = "13"/>
      <text x = 950 y = 108 font-size = 20>Iris-setosa</text>
      <circle cx = 920 cy = 150 fill = "#97D8C4" stroke = "#000" r = "13"/>
      <text x = 950 y = 158 font-size = 20>Iris-versicolor</text>
      <circle cx = 920 cy = 200 fill = "#F4B942" stroke = "#000" r = "13"/>
      <text x = 950 y = 208 font-size = 20>Iris-virginica</text>
    </g>

  <g class = "accuracy_score">
    <text font-weight = bold>Accuracy Score: {accuracy_score}%</text>
  </g>

  </svg>



<!-- <div class = "relative translate-x-36 -translate-y-3/4">
<div id = "interactive" class = "grid grid-cols-1">
    <div id = "label" class = "">
      <label id = "label">{button_label}</label>
    </div>
    <div id = "input-box" class = "w-full md:w-1/4 px-3">
      {#if valid === false}
        <input class = "shadow appearance-none border-red-700 rounded w-full py-2 px-3 text-gray-700 border leading-tight focus:outline-none focus:shadow-outline" bind:value = {k_input} type = "text" id = "k" placeholder = "Error input" />
        <p class="text-red-500 text-xs italic .p-0"> Invalid k.</p>
      {/if}
      {#if valid === true}
      <input class = "shadow appearance-none border-gray-700 rounded w-full py-2 px-3 text-gray-700 border leading-tight focus:outline-none focus:shadow-outline" bind:value = {k_input} type = "text" id = "k" placeholder = "Insert values from 1-150" />
      {/if}
    </div>
    <div class = "">
    <div id = "classify_button">
        <button bind:this={button2} class = "
          bg-white 
          hover:bg-gray-100 
          text-gray-800 
          font-semibold py-2 px-4 border 
          border-gray-400 rounded shadow
          -translate-x-full
          "
          on:click={() => {k = determine_k(k_input, k), path = path_construction(k_input, path), valid = determine_valid(k_input), update(path)}}>Classify</button>
    </div>
    </div>
  </div>
</div> -->

  <div class = " container mx-auto relative right-0 -bottom-12">
      <button bind:this = {button1} on:click = {() => {show_true = !show_true}}
        class = "
        bg-white 
        hover:bg-gray-100 
        text-gray-800 
        font-semibold py-2 px-4 border 
        border-gray-400 rounded shadow
        "
        >
        {button_t}
      </button>
  </div>
</div>

<div class = "absolute left-3/4 top-full translate-y-full pt-8">
<div id = "interactive" class = "grid grid-cols-1">
    <div id = "label" class = "relative right-32">
      <label id = "label">{button_label}</label>
    </div>
    <div id = "input-box" class = "w-full md:w-full relative right-32 py-4">
      {#if valid === false}
        <input class = "shadow appearance-none border-red-700 rounded w-full py-2 px-3 text-gray-700 border leading-tight focus:outline-none focus:shadow-outline" bind:value = {k_input} type = "text" id = "k" placeholder = "Error input" />
        <p class="text-red-500 text-xs italic p-0"> Invalid k.</p>
      {/if}
      {#if valid === true}
      <input class = "shadow appearance-none border-gray-700 rounded w-full py-2 px-3 text-gray-700 border leading-tight focus:outline-none focus:shadow-outline" bind:value = {k_input} type = "text" id = "k" placeholder = "Insert values from 1-150" />
      {/if}
    </div>
    <div class = "">
    <div id = "classify_button">
        <button bind:this={button2} class = "
          bg-white 
          hover:bg-gray-100 
          text-gray-800 
          font-semibold py-2 px-4 border 
          border-gray-400 rounded shadow
          -translate-x-full
          "
          on:click={() => {k = determine_k(k_input, k), path = path_construction(k_input, path), valid = determine_valid(k_input), update(path)}}>Classify</button>
    </div>
    </div>
  </div>
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