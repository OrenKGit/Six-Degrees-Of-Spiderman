<script>
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  export let graph;

  let svg;
  let toggleState = true; // show only the Spiderman node
  let filteredNodes;

  $: links = graph.links.map((d) => Object.create(d));
  $: nodes = graph.nodes
    .filter((node) => node.group === 0)
    .map((d) => {
      d.size = Math.pow(
        graph.links
          .filter((link) => link.source == d.id || link.target == d.id)
          .map((link) => link.value)
          .reduce((a, b) => a + b),
        2
      );
      d.connections = graph.links
        .filter((link) => link.source == d.id || link.target == d.id)
        .map((link) => 1)
        .reduce((a, b) => a + b);
      if (d.id == "u") {
        max = d.size;
        d.details.messages = max;
      }
      return Object.create(d);
    });

  function groupColour(context, d) {
    let nodesize = 2 + Math.sqrt(d.size) / 1;
    let radgrads = ["#9b5fe0", "#16a4d8", "#60dbe8", "#8bd346", "#efdf48", "#f9a52c", "#d64e12"];
    return radgrads[d.group];
  }

  let simulation;

  const radgrads = ["#9b5fe0", "#16a4d8", "#60dbe8", "#8bd346", "#efdf48", "#f9a52c"];
  const groupNames = ["Spiderman", "X-Men", "Avengers", "Fantastic Four", "Other Superheroes", "Asgardians"];
  const padding = 50;
  let svgGroup;

  const zoom = d3.zoom()
    .scaleExtent([0.1, 10])
    .on('zoom', (event) => {
      svgGroup.attr('transform', event.transform);
    });

  const numClusters = 7;
  const width = 960;
  const height = 600;

  const xScale = d3.scaleLinear().domain([0, numClusters]).range([padding, width - padding]);
  const yScale = d3.scaleLinear().domain([0, numClusters]).range([padding, height - padding]);

  const spacingFactor = 5;
  const centers = Array.from({ length: numClusters }, (_, i) => ({
    x: (i + 1) * (width / (numClusters + 1)) * spacingFactor,
    y: height / 2
  }));

  onMount(async () => {
    svgGroup = d3.select(svg).append('g');

    d3.select(svg).call(zoom);
    d3.select(svg).call(zoom.transform, d3.zoomIdentity.translate(width / 3, height / 4).scale(0.4));

    const maxNodeSize = d3.max(nodes, (d) => d.size);

    simulation = d3.forceSimulation(nodes)
      .velocityDecay(0.5)
      .force('charge', d3.forceManyBody().strength(-200).distanceMin(1).distanceMax(50))
      .force('center', d3.forceCenter(width / 2, height / 2).strength(0.05))
      .force('collide', d3.forceCollide().radius((d) => Math.sqrt(d.size / maxNodeSize) * 100 + 1));

    const node = svgGroup
      .selectAll('circle')
      .data(nodes)
      .enter()
      .append('circle')
      .attr('r', (d) => Math.sqrt(d.size / maxNodeSize) * 100)
      .style("fill", function(d) { return groupColour(this, d); })
      .style("stroke", "white") 
      .style("stroke-width", "2")
      .call(drag(simulation));

    node
      .on('mouseover', function(event, d) {
          d3.select('#tooltip')
              .style('left', (event.pageX + 10) + 'px')
              .style('top', (event.pageY - 10) + 'px')
              .style('visibility', 'visible')
              .text(d.name);
      })
      .on('mouseout', function() {
          d3.select('#tooltip')
              .style('visibility', 'hidden');
      });

    simulation.on('tick', () => {
      node
        .attr('cx', (d) => d.x)
        .attr('cy', (d) => d.y);
    });

    toggle();
  });

  function drag(simulation) {
    function dragstarted(event, d) {
      if (!event.active) simulation.alphaTarget(0.3).restart();
      d.fx = d.x;
      d.fy = d.y;
    }

    function dragged(event, d) {
      d.fx = event.x;
      d.fy = event.y;
    }

    function dragended(event, d) {
      if (!event.active) simulation.alphaTarget(0);
      d.fx = null;
      d.fy = null;
    }

    return d3.drag()
      .on('start', dragstarted)
      .on('drag', dragged)
      .on('end', dragended);
  }

  function toggle() {
    toggleState = !toggleState;
    if (toggleState) {
      // If toggleState is true, show entire Spiderman group
      filteredNodes = nodes.filter((node) => node.group === 0);
    } else {
      // If toggleState is false, show only the Spiderman node
      filteredNodes = nodes.filter((node) => node.id === "10532");
    }
    update(filteredNodes);
  }

  function update(nodes) {
    if (!simulation) return;

    svgGroup.selectAll('circle').remove();

    const maxNodeSize = d3.max(nodes, (d) => d.size);
    const node = svgGroup
      .selectAll('circle')
      .data(nodes)
      .enter()
      .append('circle')
      .attr('r', (d) => Math.sqrt(d.size / maxNodeSize) * 100)
      .style("fill", function(d) { return groupColour(this, d); })
      .call(drag(simulation));

      node
      .on('mouseover', function(event, d) {
          d3.select('#tooltip')
              .style('left', (event.pageX + 10) + 'px')
              .style('top', (event.pageY - 10) + 'px')
              .style('visibility', 'visible')
              .text(d.name);
      })
      .on('mouseout', function() {
          d3.select('#tooltip')
              .style('visibility', 'hidden');
      });

      simulation.force('charge', d3.forceManyBody().strength(-5))
      simulation.force('center', d3.forceCenter(width / 2, height / 2).strength(0.05))
      simulation.force('collide', d3.forceCollide().radius((d) => Math.sqrt(d.size / maxNodeSize) * 100 + 1));

    simulation.nodes(nodes).on('tick', () => {
      node
        .attr('cx', (d) => d.x)
        .attr('cy', (d) => d.y);
    });

    simulation.alpha(1).restart();
  }

</script>

<style>
  #tooltip {
  background-color: white;
  color: black;
  border: 1px solid black;
  padding: 3px;
  font-size: 12px;
  font-family: sans-serif;
}

.button {
    position: absolute;
    top: 150;
  }
</style>

<div id="tooltip" style="position: absolute; visibility: hidden;"></div>

<button class='button' on:click={toggle}>See Spider Group</button>

<svg bind:this={svg} width="960" height="600"></svg>
