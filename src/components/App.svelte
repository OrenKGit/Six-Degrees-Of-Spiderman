<script>
  //import BarChart from "./BarChart.svelte";
  import netdata from './NODES_LINKS.json'
  import NetworkGraph from "./NetworkGraph.svelte";
  import Graph from "./Graph.svelte";
  //import spiderman from "./spiderman!.png";
  import Scrolly from "./Scrolly.svelte";

  let value;
  const steps = [
    "<p>section1</p>",
    "<p>section2</p>",
    "<p>section3</p>",
  ];
</script>

<main>
<html lang="en">
<head>
<title>Marvel Network Graph</title>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Lato">
<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Montserrat">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
<style>
body,h1,h2,h3,h4,h5,h6 {font-family: "Lato", sans-serif}
.w3-bar,h1,button {font-family: "Montserrat", sans-serif}
.fa-anchor,.fa-coffee {font-size:200px}
</style>
</head>
<body>


<!-- Header -->
<header class="w3-container w3-center" style="padding:128px 16px;background:#e23636;color:white;">
  <h1 class="w3-margin w3-jumbo"><b>Six Degrees of Spiderman</b></h1>
  <h2 class="w3-margin">A Marvel Comics Network Graph</h2>
  <p class="w3-large"><a href="http://bioinfo.uib.es/~joemiro/marvel.html">Data Source</a></p>
</header>

<!-- Second Grid -->
<div class="w3-row-padding w3-light-grey w3-padding-64 w3-container">
  <div class="w3-content">
    <div class="w3-third w3-center">
      <!--<i class="fa fa-coffee w3-padding-64 w3-text-red w3-margin-right"></i>-->
      <!--https://upload.wikimedia.org/wikipedia/commons/thumb/b/b9/Marvel_Logo.svg/1280px-Marvel_Logo.svg.png-->
      <!--<img class="w3-padding-64 w3-text-red w3-margin-right" src={spiderman} width=400 height=500>-->
    </div>

    <div class="w3-twothird">
      <h1>Introduction</h1>
      <h5 class="w3-padding-32">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</h5>

      <p class="w3-text-grey">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Excepteur sint
        occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco
        laboris nisi ut aliquip ex ea commodo consequat.</p>
    </div>
  </div>
</div>

<!-- First Grid -->
<div class="w3-row-padding w3-padding-64 w3-container">
  <div class="w3-content">
    <div>
      <h1><centered>The Network</centered></h1>
      <div class="chart">
        <centered><NetworkGraph graph={netdata}/></centered>
        
      </div>
      <h5 class="w3-padding-32">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.</h5>

      <p class="w3-text-grey">Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Excepteur sint
        occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco
        laboris nisi ut aliquip ex ea commodo consequat.</p>
    </div>

    <div class="w3-third w3-center">
      <!--<i class="fa fa-anchor w3-padding-64 w3-text-red"></i>-->
    </div>
  </div>
</div>

<section>
  <div class="section-container">
    <div class="steps-container">
      <Scrolly bind:value>
        {#each steps as text, i}
          <div class="step" class:active={value === i}>
            <div class="step-content">{@html text}</div>
          </div>
        {/each}
        <div class="spacer" />
      </Scrolly>
    </div>
    <div class="sticky">
      <div class="chart2">
        <Graph graph={netdata} step={value}/>
      </div>
    </div>
  </div>
</section>


<div class="w3-container w3-black w3-center w3-opacity w3-padding-64">
    <h1 class="w3-margin w3-xlarge">Write Up:</h1>
</div>

</body>
</html>
</main>

<style>
  
	.chart {
		width: 1225px;
    /*max-width: 440px;*/
    height: 600px;
    float: right;
    /*
		max-width: 640px;
    height: 500px;
		/*height: calc(100% - 4em);*/
		min-height: 280px;
		max-height: 4080px;
		margin: 0 auto;
    border-style: solid;
    border: 10px;
    float: left;
	}

  .chart2 {
		width: 1000px;
    /*max-width: 440px;*/
    height: 600px;
    float: right;
    /*
		max-width: 640px;
    height: 500px;
		/*height: calc(100% - 4em);*/
		min-height: 280px;
		max-height: 4080px;
		margin: 0 auto;
    border-style: solid;
    border: 10px;
    float: left;
	}
  
  /* :global(body) {
    overflow-x: hidden;
  } */
  
  
  
  .spacer {
    height: 40vh;
  }

  /* fix scrolling */
  .sticky {
    position: sticky;
    top: 25%;
    flex: 1 1 60%;
    width: 60%;
    max-height: 75vh;
    overflow: hidden;
  }

  .section-container {

    overflow: auto; /* Add this line */
    height: 100vh; /* Add this line */
    margin-top: 1em;
    text-align: center;
    transition: background 100ms;
    display: flex;
    flex-direction: row; /* Change this line */
  }

  .step {
    height: 80vh;
    display: flex;
    place-items: center;
    justify-content: center;
  }

  .step-content {
    font-size: 1rem;
    background: whitesmoke;
    color: #ccc;
    border-radius: 5px;
    padding: .5rem 1rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    transition: background 500ms ease;
    box-shadow: 1px 1px 10px rgba(0, 0, 0, .2);
    text-align: left;
    width: 75%;
    margin: auto;
    max-width: 500px;
  }

  .step.active .step-content {
    background: white;
    color: black;
  }
  
  .steps-container,
  .sticky {
    height: 100%;
  }

  .steps-container {
    flex: 1 1 40%;
    z-index: 10;
  }
  
/* Comment out the following line to always make it 'text-on-top' */
  @media screen and (max-width: 768px) {
    .section-container {
      flex-direction: column-reverse;
    }
    .sticky {
      width: 95%;
      margin: auto;
    }
  }

</style>
