<script>

  import BarChart from "./BarChart.svelte";
  //import netdata from './nodes-links.json'
  import netdata from './10nodes.json'
  //import netdata from './marvelnodes.json'
  import data from './count-hero-network.json'
  import NetworkGraph from "./NetworkGraph.svelte";

</script>

<main>
  <!--
  <h1><center>Six Degrees of Spiderman</center></h1>
  <h3><center>A Marvel Comics Network Graph</center></h3>  
  <h4><center>
    Which Heroes are Connected the Most/Least?
    <br>
    50 Heroes with the Most/Least connections
  </center></h4>
  <div>
    <center>
      <span>Most</span>
      <label class="switch">
        <input type="checkbox">
        <span class="slider round"></span>
      </label>
      <span>Least</span>
      <br>
      <BarChart {data}/>
    </center>
  </div>
  <h4><center>Prototype Write Up:</center></h4>
  <div>
    <center>
    <b><center>What Have We Done So Far?</center></b>
    <br>
    So far we have spent the majority of our time discussing and planning out our final visualizations.
    <br>
    We spent a lot of time finding a suitable dataset to demonstrate the sociology phenomenon that we are aiming to visualize/explain.
    <br>
    We also spent considerable time attempting to build a basic network graph visualization but haven't yet succeeded.
    <br>
    Currently we have a chart showing the most connected Marvel characters that will serve as extra information
    <br>
    provided to the reader to allow them to understand the scale of character's number of connections
    <br><br>
    <b><center>What Will Be The Most Challenging Part?</center></b>
    <br>
    The most challenging part of our project will likely be implementing the expolarability onto the already challenging network graph.
    <br>
    We have already been struggling to implement the network graph and we believe our desired final implementation will require
    <br>
    a shortest path algorithm on top of animations on the network graph.
    <br>
    We plan to allow the reader to select two heroes of their choice through a dropdown menu which will then show 
    <br>
    how these two heroes are connected to eachother through comic appearances, growing edge by edge on the graph.
    </center>
  </div>
  -->
    <div class="chart">
      <NetworkGraph graph={netdata}/>
    </div>

  
</main>

<style>
  /*
  :global(body) {
    margin: 0;
    padding: 0;
  }

  :global(#map) {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  }

  input {
    display: inline-block;
    width: 100%;
    position: relative;
    margin: 0;
    cursor: pointer;
  }

  label {
    font-size: 1.5em;
    font-family: sans-serif;
    font-weight: lighter;
  }

  .switch {
    position: relative;
    display: inline-block;
    width: 60px;
    height: 34px;
  }

  /* Hide default HTML checkbox */
  /*
  .switch input {
    opacity: 0;
    width: 0;
    height: 0;
  }

  /* The slider */
  /*
  .slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #ccc;
    -webkit-transition: .4s;
    transition: .4s;
  }

  .slider:before {
    position: absolute;
    content: "";
    height: 26px;
    width: 26px;
    left: 4px;
    bottom: 4px;
    background-color: white;
    -webkit-transition: .4s;
    transition: .4s;
  }

  input:checked + .slider {
    background-color: #2196F3;
  }

  input:focus + .slider {
    box-shadow: 0 0 1px #2196F3;
  }

  input:checked + .slider:before {
    -webkit-transform: translateX(26px);
    -ms-transform: translateX(26px);
    transform: translateX(26px);
  }

  /* Rounded sliders */
  /*
  .slider.round {
    border-radius: 34px;
  }

  .slider.round:before {
    border-radius: 50%;
  }
  */
  
	.chart {
		width: 100%;
		max-width: 640px;
    height: 500px;
		/*height: calc(100% - 4em);*/
		min-height: 280px;
		max-height: 4080px;
		margin: 0 auto;
	}
  
</style>