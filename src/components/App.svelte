<script>
  import * as d3 from "d3";
  import DataPoints from "./DataPoints.svelte";
  import Iris from "./Iris.svelte";
  import WorstAcc from "./WorstAcc.svelte"
  import BestAcc from "./BestAcc.svelte"
  import ModelBreak from "./ModelBreak.svelte";
  import { onMount } from "svelte";

  let data = [];
  let iris_data = [];
  let worst_acc = [];
  let best_acc = [];
  let model_break = [];
  let button;
  let show_true = false;
  let slide_position = 0;

  $: button_t = button_text(show_true);

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

    onMount(async () => {
            let res = await fetch('k_150.csv'); 
            const csv = await res.text();
            worst_acc = d3.csvParse(csv, d3.autoType)
            console.log(worst_acc);
        });

    onMount(async () => {
            let res = await fetch('k_1.csv'); 
            const csv = await res.text();
            best_acc = d3.csvParse(csv, d3.autoType)
            console.log(best_acc);
        });
    
    onMount(async () => {
            let res = await fetch('k_99.csv'); 
            const csv = await res.text();
            model_break = d3.csvParse(csv, d3.autoType)
            console.log(model_break);
        });
      
    function button_text(status) {
      if(status === false) {
        return "Show Actual Data"
      } else if (status === true) {
        return "Hide Actual Data"
      }
    }

    $: console.log(data);
</script>

<main>
  <div height = 743 id = "slide_buttons">
    {#if slide_position !== 0}
    <button id = "prev" class = "
      bg-white 
      hover:bg-gray-100 
      text-gray-800 
      font-semibold py-2 px-4 border 
      border-gray-400 rounded shadow
      absolute bottom-8 left-8"
      on:click = {() => slide_position = slide_position - 1}> Previous </button>
    {/if}
    {#if slide_position < 6}
    <button id = "next" class = "
      bg-white 
      hover:bg-gray-100 
      text-gray-800 
      font-semibold py-2 px-4 border 
      border-gray-400 rounded shadow
      absolute bottom-8 right-8"
      on:click = {() => slide_position = slide_position + 1}> Next </button>
    {/if}
    {#if slide_position === 6}
    <button id = "try" class = "
      bg-white 
      hover:bg-gray-100 
      text-gray-800 
      font-semibold py-2 px-4 border 
      border-gray-400 rounded shadow
      absolute bottom-8 right-8"
      on:click = {() => slide_position = slide_position + 1}> Try it Yourself! </button>
    {/if}
  </div>
  {#if slide_position === 0}
    <div id='hook' class = "translate-y-80">
      <h1 class="text-2xl font-extrabold dark:text-white" style = "text-align: center; transform: translate(0, -30%)">The Importance of Moderation in Machine Learning</h1>
      <p style = "padding:10px; text-align: center;">Han Hoang, Raine Hoang</p>
    </div>
  {/if}
  {#if slide_position === 1}
    <div id='motivation' class = "translate-y-12">
      <h2 class="text-2xl font-extrabold dark:text-white" style = "text-align: left; transform: translate(0, -30%)">Our motivation</h2>
      <!-- create a bullet list -->
      <ul class="list-disc text-left">
        <li style='padding-bottom:10px'>Shows how a machine learning algorithm work</li>
        <li style='padding-bottom:10px'>K-Nearest Neighbors suits our needs to visualize the entire learning of an algorithm in one view</li>
      </ul>
    </div>
  {/if}
  <!--
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
  -->
  {#if slide_position === 2}
  <div id='intro' class = "translate-y-12">
    <h2 class="text-2xl font-extrabold dark:text-white" style = "text-align: left; transform: translate(0, -30%)">Brief Introduction to Machine Learning and K-Nearest Neighbors</h2>
    <ul class="list-disc text-left">
      <li style='padding-bottom:10px'>Machine learning happens when an algorithm learns the pattern of the data and makes predictions based on what it learned.</li>
      <li style='padding-bottom:10px'>K-Nearest Neighbors is a machine learning algorithm that learns patterns by drawing boundaries between groups of data of different classification.</li>
      <li style='padding-bottom:10px'>Boundaries are determined by the classification of all data points.</li>
      <li style='padding-bottom:10px'>In turn, a data point classification is determined by the major classification of its nearest k-size group of neighbors.</li>
    </ul>
  </div>
  {/if}

  {#if slide_position === 3}
  <div id='dataset' class = "translate-y-12">
    <h2 class="text-2xl font-extrabold dark:text-white" style = "text-align: left; transform: translate(0, -30%)">Our dataset</h2>
    <ul class="list-disc text-left">
      <!-- include a link to UC Irvine Machine Learning Repository -->
      <li style='padding-bottom:10px'>Sourced from <a href='https://archive.ics.uci.edu/dataset/53/iris'>UC Irvine Machine Learning Repository</a></li>
      <li style='padding-bottom:10px'>Contains 150 data points</li>
      <li style='padding-bottom:10px'>Contains 5 attributes: sepal length, sepal width, petal length, petal width, and class</li>
      <li style='padding-bottom:10px'><b>Outcome variable</b>: class (Iris Setosa, Iris Versicolor, and Iris Virginica)</li>
      <li style='padding-bottom:10px'><b>Predictors</b>: petal width and petal length</li>
    </ul>
    <img src = "iris.png" alt = "chart not found">
  </div>
  {/if}

  {#if slide_position === 4}
  <div id = "actual_plot" class = "translate-y-12">
    <h2 class="text-2xl font-extrabold dark:text-white" style = "text-align: left; transform: translate(0, -30%)">Data Visualization of All Data Points with Their Actual Class</h2>
    <p class = "text-left"><b>Note</b>: There are equally 50 points for each class.</p>
    <Iris {iris_data}/>
  </div>
  {/if}
<!-- 
  {#if slide_position === 5}
  <div class = "worst_acc">
    <h2 class="text-2xl font-extrabold dark:text-white" style = "text-align: left; transform: translate(0, -30%)">Worst Accuracy When k = 150</h2>
    <li style='padding-bottom:10px'>Dataset size is 150, creating an extreme case of underfit because there is no point of reference for any individual class nomination.</li>
    <li style='padding-bottom:10px'>First class appeared in the dataset is chosen to be the classification of all points as last resort.</li>
    <div class='worst_acc_plot'>
      <WorstAcc {worst_acc} />
    </div>
  </div>
  {/if}

  {#if slide_position === 6}
  <div class = "grid grid-cols-2 gap-4">
    <div id = "worst_acc">
      <h2 class="text-2xl font-extrabold dark:text-white" style = "text-align: left; transform: translate(0, -30%)">Worst Accuracy When k = 150</h2>
      <li class = "text-left scroll-pb-2">Dataset size is 150, creating an extreme case of underfit because there is no point of reference for any individual class nomination.</li>
      <li class = "text-left scroll-pb-2">First class appeared in the dataset is chosen to be the classification of all points as last resort.</li>
      <div id='worst_acc_plot' class = "scroll-pb-2">
        <WorstAcc {worst_acc} />
      </div>
    </div>
    <div id = "best_acc">
      <h2 class="text-2xl font-extrabold dark:text-white" style = "text-align: left; transform: translate(0, -30%)"> Best Accuracy When k = 1</h2>
      <li style='padding-bottom:10px'>A data point can only reference itself for classification, creating an extreme case of overfit.</li>
      <li style='padding-bottom:10px'>For every data point, actual classification = predicted classification</li>
      <div class='best_acc_plot'>
        <BestAcc {best_acc} />
      </div>
    </div>
  </div>
  {/if} -->

  {#if slide_position === 5}
  <div class = "grid grid-cols-3 gap-4 translate-y-12">
    <div id = "worst_acc_title">
      <h2 class="text-2xl font-extrabold dark:text-white">Worst Accuracy When k = 150</h2>
    </div>

    <div id = "best_acc_title">
      <h2 class="text-2xl font-extrabold dark:text-white"> Best Accuracy When k = 1</h2>
    </div>

    <div id = "generalized_title">
      <h2 class="text-2xl font-extrabold dark:text-white">Generalized When k = 99</h2>
    </div>

    <div id = "worst_acc_text">
      <li style='padding-bottom:10px' class = "text-left">Dataset size is 150, creating an extreme case of underfit because there is no point of reference for any individual class nomination.</li>
      <li style='padding-bottom:10px' class = "text-left">First class appeared in the dataset is chosen to be the classification of all points as last resort.</li> 
    </div>

    <div id = "best_acc_text">
      <li style='padding-bottom:10px' class = "text-left">A data point can only reference itself for classification, creating an extreme case of overfit.</li>
      <li style='padding-bottom:10px' class = "text-left">For every data point, actual classification = predicted classification</li>
    </div>

    <div id = "generalized_text">
      <li style='padding-bottom:10px' class = "text-left">KNN generalizes the data when k is in the middle range.</li>
      <li style='padding-bottom:10px' class = "text-left">Errors made with misclassified data points when the model 
        is generalized are generally forgivable because those points are typically borderline 
        between 2 classes. One flower may very well be of another type if we have a slightly different dataset.</li>
    </div>

    <div id = "worst_acc_plot">
      <WorstAcc {worst_acc} {show_true} />
    </div>

    <div id = "best_acc_plot">
      <BestAcc {best_acc} {show_true} />
    </div>

    <div id = "generalized_plot">
      <ModelBreak {model_break} {show_true} />
    </div>


    <!-- <div id = "worst_acc">
      <h2 class="text-2xl font-extrabold dark:text-white" style = "text-align: left; transform: translate(0, -30%)">Worst Accuracy When k = 150</h2>
      <li style='padding-bottom:10px'>Dataset size is 150, creating an extreme case of underfit because there is no point of reference for any individual class nomination.</li>
      <li style='padding-bottom:10px'>First class appeared in the dataset is chosen to be the classification of all points as last resort.</li> 
      <div class='worst_acc_plot'>
        <WorstAcc {worst_acc} />
      </div>
    </div>
    <div id = "best_acc">
      <h2 class="text-2xl font-extrabold dark:text-white" style = "text-align: left; transform: translate(0, -30%)"> Best Accuracy When k = 1</h2>
      <li style='padding-bottom:10px'>A data point can only reference itself for classification, creating an extreme case of overfit.</li>
      <li style='padding-bottom:10px'>For every data point, actual classification = predicted classification</li>
      <div class='best_acc_plot'>
        <BestAcc {best_acc} />
      </div>
    </div>
    <div id = "generalized">
      <h2 class="text-2xl font-extrabold dark:text-white" style = "text-align: left; transform: translate(0, -30%)">Generalized When k = 99</h2>
      <li style='padding-bottom:10px'>KNN generalizes the data when k is in the middle range.</li>
      <li style='padding-bottom:10px'>Errors made with misclassified data points when the model is generalized are generally forgivable because those points are typically borderline between 2 classes. One flower may very well be of another type if we have a slightly different dataset.</li>
      <div class='generalized_plot'>
        <ModelBreak {model_break} />
      </div>
    </div> -->
  </div>

  <div id = "legend" class = "-translate-x-20 translate-y-6">
    <svg width = "1042" id = "legend" stroke = "#000">
      <circle cx = 500 cy = 90 fill = "#4059AD" stroke = "#000" r = "13"/>
      <text x = 530 y = 98 font-size = 20>Iris-setosa</text>
      <circle cx = 700 cy = 90 fill = "#97D8C4" stroke = "#000" r = "13"/>
      <text x = 730 y = 98 font-size = 20>Iris-versicolor</text>
      <circle cx = 930 cy = 90 fill = "#F4B942" stroke = "#000" r = "13"/>
      <text x = 960 y = 98 font-size = 20>Iris-virginica</text>
    </svg>
  </div>

  <div id = "true_button" class = "-translate-y-16">
      <button bind:this = {button} on:click = {() => {show_true = !show_true}} 
      class = "
      bg-white 
      hover:bg-gray-100 
      text-gray-800 
      font-semibold py-2 px-4 border 
      border-gray-400 rounded shadow
      translate-y-20"
      >
      {button_t}
      </button>
  </div>
  {/if}

  {#if slide_position === 7}
  <div id = "interaction" class = "translate-y-12">
    <h2 class="text-2xl font-extrabold dark:text-white" style = "text-align: left; transform: translate(0, -30%)">Try It Out For Yourself</h2>
    <div class='interaction_plot'>
      <DataPoints {data}/>
    </div>
  </div>
  {/if}

  {#if slide_position === 6}
  <div class='summary translate-y-12' style = "padding-bottom: 0;">
    <h2 class = "text-2xl font-extrabold dark:text-white pb-2">Takeaways</h2>
    <li class = "pb-3">Low k values overfit</li>
    <li class = "pb-3">High k values underfit</li>
    <li class = "pb-3">Generalized when k is not too low or too high</li>
    <li>Best accuracy score != best model</li>
  </div>
  {/if}
  <!-- <div class = "k-button">
    <button id = "button3" on:click = {update_k1} on:click = {() => k = 1}> 1 </button>
    <button id = "button1" on:click = {update_k3} on:click = {() => k = 3}> 3 </button>
    <button id = "button2" on:click = {update_k150} on:click = {() => k = 150}> 150 </button>
  </div> -->
  <!-- <div class = "label">
    <label id = "label">{button_label}</label>
  </div>
  <div class = "input">
    <input bind:value = {k_input} type = "text" id = "k" placeholder = "Insert values from 1-150" />
    <button bind:this={button} on:click={() => {k = determine_k(k_input, k), path = path_construction(k_input, path), valid = determine_valid(k_input), update(path)}}>Submit</button>
  </div>
  <div class = "valid">
    {#if valid === false}
      <p>Not a valid k</p>
    {/if}
  </div> -->
  <!--
  <div class = "write-up">
    <h1 style = "text-align:center; margin-top:100px"><u>Write-Up</u></h1>
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
  -->
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
  /* #button1, #button2, #button3 {
    transform: translate(0, 150%);
    width: 40px;
  }
  #k-button{
      text-align: center;
  } */
  button:hover {
    cursor: pointer;
  }
  .label {
    transform: translate(0, 220%);
    text-align: center;
  }
  .iris_plot {
    margin: 50px;
  }
  .input {
    transform: translate(0, 200%);
  }
  .valid {
    transform: translate(0, 220%);
    color: red;
    font-weight: bold;
  }
  .worst_acc, .model_break, .best_acc, .interaction {
    padding-bottom: 10px;
    padding-top: 10px;
  } 

  .hook {
    text-align: center;
    padding-top: 250px;
    padding-bottom: 500px;
  }

  .motivation {
    text-align: left;
    padding-bottom: 500px;
  }

  .intro {
    text-align: left;
    padding-bottom: 500px;
  }

  .dataset {
    text-align: left;
    padding-bottom: 500px;
  }

  .actual_plot {
    text-align: left;
    padding-bottom: 500px;
  }

  .worst_acc {
    text-align: left;
    padding-bottom: 500px;
  }

  .worst_acc_plot {
    padding: 30px;
    text-align: center;
  }

  .generalized {
    text-align: left;
    padding-bottom: 500px;
  }

  .generalized_plot {
    padding: 30px;
    text-align: center;
  }

  .best_acc {
    text-align: left;
    padding-bottom: 500px;
  }

  .best_acc_plot {
    padding: 30px;
    text-align: center;
  }

  .interaction {
    text-align: left;
    padding-bottom: 500px;
  }

  .interaction_plot {
    padding: 30px;
    text-align: center;
  }

  .summary {
    text-align: left;
    padding-bottom: 500px;
  }
</style>
