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
  <p style = "padding:10px; text-align: left;">Within machine learning, there are 3 main subtypes. The first one is supervised learning, which involves using labeled data in order to making predictions. 
    In contrast, the second type of machine learning uses unlabeled data and is called unsupervised learning. Finally, the last type of machine learning is called reinforcement learning and consists 
    of the machine figuring out the best decisions through trial and error. In this article, we are going to focus on the first type of machine learning and look at a specific algorithm that is used to 
    classify unseen data, K-Nearest Neighbors or otherwise known as KNN.
  </p>
  <p style = "padding:10px; text-align: left;">
    The goal of classification algorithms is to categorize unseen data based on the patterns seen within the training data. KNN is a type of classification algorithm that uses the distance 
    between data points to classify new data. 
  </p>
  <DataPoints {data}/>
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
  }
  .write-up {
      text-align:left;
      padding: 10px;
    }
</style>
