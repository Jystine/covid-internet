<!-- <script>
    import * as d3 from "d3";
    import KNN from "ml-knn";
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
    let k = 3;
    let show_true = false;
    let button;
    let feature = "petals"
    $: petal_features = create_petal_features(data);
    $: sepal_features = create_sepal_features(data);
    $: classes = create_classes(data);

    $: petal_knn = create_knn(petal_features, classes, k)
    $: sepal_knn = create_knn(sepal_features, classes, k)

    $: petal_predicted = create_predictions(petal_features, petal_knn);
    $: sepal_predicted = create_predictions(sepal_features, sepal_knn)

    $: petal_data_class = features_prediction(petal_predicted);
    $: sepal_data_class = features_prediction(sepal_predicted)

    $: petal_width_max = d3.max(data, (d) => d.petal_width);
    $: petal_length_max = d3.max(data, (d) => d.petal_length);
    $: sepal_length_max = d3.max(data, (d) => d.sepal_length);
    $: sepal_width_max = d3.max(data, (d) => d.sepal_width);
    $: petal_width_area = create_area(100, petal_width_max);
    $: petal_length_area = create_area(100, petal_length_max);
    $: sepal_length_area = create_area(100, sepal_length_max);
    $: sepal_width_area = create_area(100, sepal_width_max);
    $: accuracy_score = accuracy(data, petal_data_class)
    $: button_t = button_text(show_true);
    // $: data_class = classification(data, k);
    $: slider_label = `k = ${k}`;
    // $: classified_area = classify_area(data, petal_width_area, petal_length_area, k)

    $: classified_petal_area = classify_boundaries(petal_knn, petal_width_area, petal_length_area);
    $: classified_sepal_area = classify_boundaries(sepal_knn, sepal_width_area, sepal_length_area);

    $: console.log(sepal_width_max)

    $: petal_x = d3
    .scaleLinear()
    .domain([0, d3.max(data, (d) => d.petal_width)])
    .range([marginLeft, width - marginRight])

    $: petal_y = d3
    .scaleLinear()
    .domain([0, d3.max(data, (d) => d.petal_length)])
    .range([height - marginBottom, marginTop])

    $: sepal_x = d3
    .scaleLinear()
    .domain([0, d3.max(data, (d) => d.sepal_width)])
    .range([marginLeft, width - marginRight])

    $: sepal_y = d3
    .scaleLinear()
    .domain([0, d3.max(data, (d) => d.sepal_length)])
    .range([height - marginBottom, marginTop])

    $: create_axis(feature, gx, gy, petal_x, petal_y, sepal_x, sepal_y);

    $: max = d3.max(data, (d) => Math.abs(d.sepal_length))
    $: min = d3.min(data, (d) => d.sepal_length)

    $: color = d3
      .scaleSequential()
      .domain([min, max])
      .interpolator(d3.interpolateViridis)

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

      function create_area(range, max) {
        let area_lst = []
        for (let i = 0; i <= max; i = i + max/range) {
          area_lst.push(parseFloat(i.toFixed(3)));
        }
        return area_lst;
      }

      function accuracy(data, data_class) {
        let correct = 0;
        for (let i = 0; i < data_class.length; i++) {
          if (data[data_class[i][0]].class === data_class[i][1]) {
            correct = correct + 1;
          }
        }
        return Math.round((correct / data_class.length).toFixed(2) * 100);
      }

      function button_text(status) {
        if(status === false) {
          return "Show Actual Data"
        } else if (status === true) {
          return "Hide Actual Data"
        }
      }

      function create_petal_features(data) {
        let petal_features = []
        for (let i = 0; i < data.length; i++) {
          let features = [data[i].petal_width, data[i].petal_length]
          petal_features.push(features)
        }
        return petal_features
      }

      function create_sepal_features(data) {
        let sepal_features = [];
        for (let i = 0; i < data.length; i++) {
          let features = [data[i].sepal_width, data[i].sepal_length]
          sepal_features.push(features);
        }
        return sepal_features
      }

      function create_classes(data) {
        let classes = [];
        for (let i = 0; i < data.length; i++) {
          classes.push(data[i].class)
        }
        return classes
      }

      function create_knn(training_data, classes, k) {
        if (training_data.length !== 0) {
          return new KNN(training_data, classes, {k: k})
        }
      }

      function create_predictions(new_data, model) {
        if (model !== undefined) {
          return model.predict(new_data);
        }
      }

      function features_prediction(predictions) {
        //Maps the data to the prediction
        let data_class = {}
        if (predictions !== undefined) {
          for (let i = 0; i < predictions.length; i++) {
            data_class[i] = predictions[i]
          }
        }
        var result =  Object.keys(data_class).map(function(key) {
            return [key, data_class[key]];
          });
        return result;
      }

      function classify_boundaries(model, area1, area2) {
        let result = {};
        let idx = 0;
        if (area1 !== undefined && area2 !== undefined && model !== undefined) {
          for (let i = 0; i < area1.length; i++) {
            for (let j = 0; j < area2.length; j++) {
              let point = [area1[i], area2[j]]
              result[idx] = {x: point[0], y: point[1], class: model.predict(point)}
              idx = idx + 1;
            }
          }
          var result_items =  Object.keys(result).map(function(key) {
            return [key, result[key]];
          });
          return result_items;
        }
      }

      function create_axis(feature, gx, gy, petal_x, petal_y, sepal_x, sepal_y) {
        if (feature === "petals") {
          d3.select(gx).call(d3.axisBottom(petal_x))
          d3.select(gy).call(d3.axisLeft(petal_y))
        } else if (feature === "sepals") {
          d3.select(gx).call(d3.axisBottom(sepal_x))
          d3.select(gy).call(d3.axisLeft(sepal_y))
        }
      }

      // function classify_area(data, area1, area2, k) {
      //   let result = {}
      //   let distance;
      //   let idx = 0;
      //   for (let i = 0; i < area1.length; i++) {
      //     for (let j = 0; j < area2.length; j++) {
      //       let distance_dict = {}
      //       let classes = {'Iris-virginica': 0, 'Iris-setosa': 0, "Iris-versicolor": 0};
      //       let point = [area1[i], area2[j]]
      //       for (let d = 0; d < data.length; d++) {
      //         distance = Math.sqrt(((data[d].petal_width - point[0]) ** 2) + ((data[d].petal_length - point[1]) ** 2))
      //         distance_dict[d] = distance;
      //       }
      //       var dist_items = Object.keys(distance_dict).map(function(key) {
      //       return [key, distance_dict[key]];
      //       });
      //       dist_items.sort(function(first, second) {
      //         return first[1] - second[1];
      //       });
      //       for (let n = 0; n < k; n++){
      //       classes[data[dist_items[n][0]].class] = classes[data[dist_items[n][0]].class] + 1;
      //       }
      //       var class_items = Object.keys(classes).map(function(key) {
      //         return [key, classes[key]];
      //       });
      //       class_items.sort(function(first, second) {
      //         return second[1] - first[1];
      //       });
      //       result[idx] = {x: point[0], y: point[1], class: class_items[0][0]}
      //       idx = idx + 1;
      //     }
      //   }
      //   var result_items =  Object.keys(result).map(function(key) {
      //       return [key, result[key]];
      //     });
      //   return result_items;
      // }

      // function classification(data, k) {
      //   let result = {}
      //   let distance;
      //   for (let i = 0; i < data.length; i++) {
      //     let distance_dict = {};
      //     let classes = {'Iris-virginica': 0, 'Iris-setosa': 0, "Iris-versicolor": 0};
      //     for (let j = 0; j < data.length; j++) {
      //       if (i === j) {
      //         continue;
      //       } else {
      //         distance = Math.sqrt(((data[i].petal_length - data[j].petal_length) ** 2) + ((data[i].petal_width - data[j].petal_width) ** 2))
      //         distance_dict[j] = distance;
      //       }
      //     }
      //     var dist_items = Object.keys(distance_dict).map(function(key) {
      //       return [key, distance_dict[key]];
      //     });
      //     dist_items.sort(function(first, second) {
      //       return first[1] - second[1];
      //     });
      //     for (let n = 0; n < k; n++){
      //       classes[data[dist_items[n][0]].class] = classes[data[dist_items[n][0]].class] + 1;
      //     }
      //     var class_items = Object.keys(classes).map(function(key) {
      //       return [key, classes[key]];
      //     });
      //     class_items.sort(function(first, second) {
      //       return second[1] - first[1];
      //     });
      //     result[i] = class_items[0][0]
      //     }
      //     var result_items =  Object.keys(result).map(function(key) {
      //       return [key, result[key]];
      //     });
      //     return result_items;
      // }

      $: console.log(data);
      $: console.log(feature);

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
        {#if d.class === "Iris-setosa"}
          <circle key = {i} cx = {x(d.petal_width)} cy = {y(d.petal_length)} fill = {color(d.sepal_length)} stroke = "#000" r = {circleSize(d.sepal_width)}/>
        {/if}
        {#if d.class === "Iris-versicolor"}
          <rect key = {i} width = {rectangleSize(d.sepal_width)} height = {rectangleSize(d.sepal_width)} x = {x(d.petal_width)} y = {y(d.petal_length)} fill = {color(d.sepal_length)} stroke = "#000"/>
        {/if}
        {#if d.class === "Iris-virginica"}
          <g transform = {`translate(${x(d.petal_width)}, ${y(d.petal_length)})`}>
            <path d="M{triangleSize(d.sepal_width)/4},{triangleSize(d.sepal_width)/2} h{triangleSize(d.sepal_width)/2} l{-triangleSize(d.sepal_width)/4},{-triangleSize(d.sepal_width)/2} Z" fill = {color(d.sepal_length)} stroke = "#000"/>
          </g>
        {/if}
      {/each}
    {/if}

            $(document).ready(function(){
            console.log(data[0])
        }); 

    <g class = "boundary_lines">
    {#if classified_petal_area !== undefined && feature === "petals"}
      {#each classified_petal_area as a}
        {#if a[1].class === "Iris-setosa"}
            <rect key = a[0] width = 10 height = 10 x = {petal_x(a[1].x)} y = {petal_y(a[1].y) - 10} fill = "#4059AD" r = "5"/>
        {/if}
        {#if a[1].class === "Iris-versicolor"}
            <rect key = a[0] width = 10 height = 10 x = {petal_x(a[1].x)} y = {petal_y(a[1].y) - 10} fill = "#97D8C4" r = "5"/>
        {/if}
        {#if a[1].class === "Iris-virginica"}
            <rect key = a[0] width = 10 height = 10 x = {petal_x(a[1].x)} y = {petal_y(a[1].y) - 10} fill = "#F4B942" r = "5"/>
        {/if}
      {/each}
    {/if}
    {#if classified_sepal_area !== undefined && feature === "sepals"}
      {#each classified_sepal_area as a}
        {#if a[1].class === "Iris-setosa"}
            <rect key = a[0] width = 10 height = 10 x = {sepal_x(a[1].x)} y = {sepal_y(a[1].y) - 10} fill = "#4059AD" r = "5"/>
        {/if}
        {#if a[1].class === "Iris-versicolor"}
            <rect key = a[0] width = 10 height = 10 x = {sepal_x(a[1].x)} y = {sepal_y(a[1].y) - 10} fill = "#97D8C4" r = "5"/>
        {/if}
        {#if a[1].class === "Iris-virginica"}
            <rect key = a[0] width = 10 height = 10 x = {sepal_x(a[1].x)} y = {sepal_y(a[1].y) - 10} fill = "#F4B942" r = "5"/>
        {/if}
      {/each}
    {/if}
    </g>

    <g class = "points" style = "position: absolute; z-index: 2;">
    {#if petal_data_class !== undefined && feature === "petals"}
      {#each petal_data_class as d}
        {#if show_true == false}
          {#if d[1] === "Iris-setosa"}
            <circle key = {d[0]} cx = {petal_x(data[d[0]].petal_width)} cy = {petal_y(data[d[0]].petal_length)} fill = "#4059AD" stroke = "#000" r = "5"/>
          {/if}
          {#if d[1] === "Iris-versicolor"}
            <circle key = {d[0]} cx = {petal_x(data[d[0]].petal_width)} cy = {petal_y(data[d[0]].petal_length)} fill = "#97D8C4" stroke = "#000" r = "5"/>
          {/if}
          {#if d[1] === "Iris-virginica"}
            <circle key = {d[0]} cx = {petal_x(data[d[0]].petal_width)} cy = {petal_y(data[d[0]].petal_length)} fill = "#F4B942" stroke = "#000" r = "5"/>
          {/if}
        {/if}
        {#if show_true == true}
          {#if data[d[0]].class === "Iris-setosa"}
            <circle key = {d[0]} cx = {petal_x(data[d[0]].petal_width)} cy = {petal_y(data[d[0]].petal_length)} fill = "#4059AD" stroke = "#000" r = "5"/>
          {/if}
          {#if data[d[0]].class === "Iris-versicolor"}
            <circle key = {d[0]} cx = {petal_x(data[d[0]].petal_width)} cy = {petal_y(data[d[0]].petal_length)} fill = "#97D8C4" stroke = "#000" r = "5"/>
          {/if}
          {#if data[d[0]].class === "Iris-virginica"}
            <circle key = {d[0]} cx = {petal_x(data[d[0]].petal_width)} cy = {petal_y(data[d[0]].petal_length)} fill = "#F4B942" stroke = "#000" r = "5"/>
          {/if}
        {/if}
      {/each}
    {/if}
    {#if sepal_data_class !== undefined && feature === "sepals"}
      {#each sepal_data_class as d}
        {#if show_true == false}
          {#if d[1] === "Iris-setosa"}
            <circle key = {d[0]} cx = {sepal_x(data[d[0]].sepal_width)} cy = {sepal_y(data[d[0]].sepal_length)} fill = "#4059AD" stroke = "#000" r = "5"/>
          {/if}
          {#if d[1] === "Iris-versicolor"}
            <circle key = {d[0]} cx = {sepal_x(data[d[0]].sepal_width)} cy = {sepal_y(data[d[0]].sepal_length)} fill = "#97D8C4" stroke = "#000" r = "5"/>
          {/if}
          {#if d[1] === "Iris-virginica"}
            <circle key = {d[0]} cx = {sepal_x(data[d[0]].sepal_width)} cy = {sepal_y(data[d[0]].sepal_length)} fill = "#F4B942" stroke = "#000" r = "5"/>
          {/if}
        {/if}
        {#if show_true == true}
          {#if data[d[0]].class === "Iris-setosa"}
            <circle key = {d[0]} cx = {sepal_x(data[d[0]].sepal_width)} cy = {sepal_y(data[d[0]].sepal_length)} fill = "#4059AD" stroke = "#000" r = "5"/>
          {/if}
          {#if data[d[0]].class === "Iris-versicolor"}
            <circle key = {d[0]} cx = {sepal_x(data[d[0]].sepal_width)} cy = {sepal_y(data[d[0]].sepal_length)} fill = "#97D8C4" stroke = "#000" r = "5"/>
          {/if}
          {#if data[d[0]].class === "Iris-virginica"}
            <circle key = {d[0]} cx = {sepal_x(data[d[0]].sepal_width)} cy = {sepal_y(data[d[0]].sepal_length)} fill = "#F4B942" stroke = "#000" r = "5"/>
          {/if}
        {/if}
      {/each}
    {/if}
  </g>

    <text x = 420 
    y = 530 
    font-size = 20>Petal Width</text>
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

<div class = "overlay">
  <div class = "slider_class">
    <label>{slider_label}</label>
    <input
      id = "slider"
      type = "range"
      min = "1"
      max = "150"
      bind:value = {k}
    />
  </div>
  <div class = "button">
    <button bind:this = {button} on:click = {() => {show_true = !show_true}} style = "margin: 10px">{button_t}</button>
  </div>
</div>

<div class = "dropdown">
  <label for = "features">Features:</label>
  <select name = "features" id = "features" bind:value = {feature}>
    <option value = "petals">Petals</option>
    <option value = "sepals">Sepals</option>
  </select>
</div>

<style>
  .points {
    fill-opacity: 100%;
    stroke-opacity: 100%;
    filter: brightness(75%);
  }
  button {
    margin-bottom: 1em;
  }
  .overlay {
    transform: translate(0, 70%);
  }
  .dropdown {
    transform: translate(10%, 0);
  }
</style> -->