<script>
  import * as d3 from "d3";
  import DataPoints from "./DataPoints.svelte";
  import Iris from "./Iris.svelte";
  import { onMount } from "svelte";

  let data = [];
  let k = 3;
  let iris_data = [];
  $: button_label = `k = ${k}`;

    onMount(async () => {
            let res = await fetch('k_3.csv'); 
            const csv = await res.text();
            data = d3.csvParse(csv, d3.autoType)
            console.log(data);
        });

    onMount(async () => {
            let res = await fetch('iris.csv'); 
            const csv = await res.text();
            iris_data = d3.csvParse(csv, d3.autoType)
            console.log(iris_data);
        });

    async function update_k3() {
      const res = await fetch('k_3.csv');
      const csv = await res.text();
      data = d3.csvParse(csv, d3.autoType)
      console.log(data);
    }

    async function update_k150() {
      const res = await fetch('k_150.csv');
      const csv = await res.text();
      data = d3.csvParse(csv, d3.autoType)
      console.log(data);
    }

    async function update_k1() {
      const res = await fetch('k_1.csv');
      const csv = await res.text();
      data = d3.csvParse(csv, d3.autoType)
      console.log(data);
    }
</script>

<main>
  <h1>KNN Machine Learning</h1>
  <p style = "padding:10px; text-align: left;">Within machine learning, there are 3 main subtypes. The first one is supervised learning, which involves using labeled data in order to making predictions. 
    In contrast, the second type of machine learning uses unlabeled data and is called unsupervised learning. Finally, the last type of machine learning is called reinforcement learning and consists 
    of the machine figuring out the best decisions through trial and error. In this article, we are going to focus on the first type of machine learning and look at a specific algorithm that is used to 
    classify unseen data, K-Nearest Neighbors or otherwise known as KNN.
  </p>
  <p style = "padding:10px; text-align: left;">
    The goal of classification algorithms is to categorize unseen data based on the patterns seen within the training data. KNN is a type of classification algorithm that uses the distance 
    between data points to classify new data. Essentially, when you input a data point, KNN calculates the distance between that point and every other point in the dataset. Afterwards, it finds the 
    k closest points and takes a majority vote by looking at the most common label between them. For example, if k is equal to 3, the algorithm will look for the 3 data points that 
    have the shortest distance between them and the inputted data point then take a look at their labels and assign the most seen label to the inputted data point. 
  </p>
  <p style = "padding:10px; text-align: left;">
    In this article, we will be using the Iris dataset to walk through the process and visualize KNN. The Iris dataset was created back in 1936 and is one of the oldest datasets used to demonstrate 
    classification methods. This dataset consists of 5 columns: petal length, petal width, sepal length, sepal width, and class. The thing we are trying to predict whether a flower is of class Iris Setosa, Iris 
    Versicolor, or Iris Virginica and we are going to be using a combination of petal length and petal width to classify that flower.
  </p>
  <img src = "iris.png" alt = "chart not found">
  <div class = "plot">
    <Iris {iris_data}/>
    <DataPoints {data}/>
  </div>
  <div class = "k-button">
    <button id = "button3" on:click = {update_k1} on:click = {() => k = 1}> 1 </button>
    <button id = "button1" on:click = {update_k3} on:click = {() => k = 3}> 3 </button>
    <button id = "button2" on:click = {update_k150} on:click = {() => k = 150}> 150 </button>
  </div>
  <div class = "label">
    <label id = "label">{button_label}</label>
  </div>
  <p style = "padding:30px;">Write your HTML here</p>
  <div class = "write-up">
    <h1 style = "text-align:center"><u>Write-Up</u></h1>
    <p>We have visually encoded data points with different features. We also set up a legend and an introduction writeup for our website. For interactive visualization, 
      we have a working interactive that demonstrates the mechanisms of KNN algorithm in machine learning via tuning the hyperparameter k, the amount of nearest neighbors 
      that contributes to the vote of the classified category of a data point. We did this by drawing boundary lines between areas of different classes that correspond with 
      different colors and calculating the proportion of matching class between predicted and actual data. With our current implementation, the viewers of our website can 
      adjust the k hyperparameter to different values and perceive, in a real-time manner, the changes in the accuracy level as well as changes in the determined areas of 
      different data classes.
    </p>
    <p>
      The most challenging of our project so far is to code KNN algorithm in Javascript. We had to wrap our head around the logic of KNN, like calculate the distance and 
      keep track of the computed distance and the associated data points with their class, and translate that to Javascript. We also had to come up with a way to encode 
      the boundary lines between different classes; that is to divide the visualization space into grids and color each grid with the color of the classified class that 
      the grid area belongs to. Our implementation works with a heavy overhead, so our next big challenge is to optimize our code to make the interaction in our visualization 
      more smooth.
    </p>
  </div>
</main>

<style>
  main {
    text-align: center;
    font-family: 'Open Sans', sans-serif;
    margin-left: 100px;
    margin-right: 100px;
  }
  .write-up {
      text-align:left;
      padding: 10px;
    }
  #button1, #button2, #button3 {
    transform: translate(0, 150%);
    width: 40px;
  }
  #k-button{
      text-align: center;
  }
  button:hover {
    cursor: pointer;
  }
  .label {
    transform: translate(0, 220%);
    text-align: center;
  }
</style>
