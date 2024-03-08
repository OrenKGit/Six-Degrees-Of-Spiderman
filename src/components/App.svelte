<script>
  import BarChart from "./BarChart.svelte";
  import netdata from './NODES_LINKS.json'
  import NetworkGraph from "./NetworkGraph.svelte";
</script>

<main>
    <div class="chart">
      <NetworkGraph graph={netdata}/>
    </div>
</main>

<style>
  
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