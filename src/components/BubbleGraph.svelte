<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    export let graph;
  
    let svg;
    let simulation;
    let isNodesRandomized = true;
    


    $: links = graph.links.map((d) => Object.create(d));
    $: nodes = graph.nodes.map((d) => {
        d.size = Math.pow(graph.links
        .filter((link) => link.source == d.id || link.target == d.id)
        .map((link) => link.value)
        .reduce((a, b) => a + b), 2);
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
        let radgrads = ["#9b5fe0", "#16a4d8", "#60dbe8", "#8bd346", "#efdf48", "#f9a52c",  "#d64e12"];
        return radgrads[d.group];
    }

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

    const spacingFactor = 5; // Adjust this value to increase or decrease spacing
    const centers = [
    { x: width / 5, y: height / 7 }, // Top left
    { x: 4 * width / 5, y: height / 7 }, // Top right
    { x: width / 5, y: 2 * height / 2 }, // Middle left
    { x: 4 * width / 5, y: 2* height / 2 }, // Middle right
    { x: width / 5, y: 12 * height / 7 }, // Bottom left
    { x: 4 * width / 5, y: 12 * height / 7 }, // Bottom right
];
    // add transition from random to grouped view
    // add legend for group categories
    onMount(async () => {

        svgGroup = d3.select(svg).append('g'); 

        d3.select(svg).call(zoom);
        d3.select(svg).call(zoom.transform, d3.zoomIdentity.translate(width / 4, height / 5).scale(0.4));

      const maxNodeSize = d3.max(nodes, d => d.size);
  
      simulation = d3.forceSimulation(graph.nodes)
        .velocityDecay(0.5)
        .force('charge', d3.forceManyBody().strength(-100).distanceMin(100).distanceMax(200))
        .force('center', d3.forceCenter(width / 2, height / 2))
        .force('collide', d3.forceCollide().radius(d => Math.sqrt(d.size / maxNodeSize) * 100))
        .force('x', d3.forceX(width / 2).strength(0.2)) 
        .force('y', d3.forceY(height / 2).strength(0.2));
        // .force('x', d3.forceX(d => centers[d.group].x).strength(1)) 
        // .force('y', d3.forceY(d => centers[d.group].y).strength(1)) 

        const node = svgGroup
        .selectAll('circle')
        .data(graph.nodes)
        .enter()
        .append('circle')
        .attr('r', d =>  Math.sqrt(d.size / maxNodeSize) * 100)
        .style("fill", function(d) { return groupColour(this, d); })
        .style("stroke", "white") 
        .style("stroke-width", "1")
        .call(drag(simulation));
  
      node.append('title')
        .text(d => d.name);
  
      simulation.on('tick', () => {
        node
          .attr('cx', d => d.x)
          .attr('cy', d => d.y);
      });

    //   svgGroup.selectAll('.label')
    // .data(groupNames)
    // .enter()
    // .append('rect')
    // .attr('x', (d, i) => centers[i].x - 70) 
    // .attr('y', (d, i) => centers[i].y - 50) 
    // .attr('width', 140)
    // .attr('height', 40) 
    // .style('fill', 'white');

// svgGroup.selectAll('.label')
//     .data(groupNames)
//     .enter()
//     .append('text')
//     .attr('class', 'label')
//     .attr('x', (d, i) => centers[i].x)
//     .attr('y', (d, i) => centers[i].y- 30) 
//     .text(d => d)
//     .style('text-anchor', 'middle')
//     .style('font-size', '35px'); 

      const legend = svgGroup.selectAll('.legend')
            .data(radgrads)
            .enter()
            .append('g')
            .attr('class', 'legend')
            .attr('transform', (d, i) => `translate(0,${i * 40})`);

        legend.append('rect')
            .attr('x', width + 450)
            .attr('width', 50)
            .attr('height', 50)
            .style('fill', d => d);

        legend.append('text')
            .attr('x', width + 444)
            .attr('y', 20)
            .attr('dy', '.35em')
            .style('text-anchor', 'end')
            .style('font-size', '35px') 
            .text((d, i) => groupNames[i]);

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
    if (isNodesRandomized) { 
      // Regroup nodes into their clusters
      simulation.force('x', d3.forceX(d => centers[d.group].x).strength(0.3));
      simulation.force('y', d3.forceY(d => centers[d.group].y).strength(0.3));
      simulation.alpha(1).restart();
      simulation.force('x').initialize(graph.nodes);
      simulation.force('y').initialize(graph.nodes);
      isNodesRandomized = false;
    } else {
      // Randomize nodes into one cluster
      simulation.force('x', d3.forceX(width / 2).strength(0.2));
      simulation.force('y', d3.forceY(height / 2).strength(0.2));
      simulation.alpha(1).restart();
      simulation.force('x').initialize(graph.nodes);
      simulation.force('y').initialize(graph.nodes);
      isNodesRandomized = true;
    }
  }

    
  </script>

<div class="checkbox-container">
  <input type="checkbox" on:change={toggle}>
  <span>Group</span>
</div>
  <svg bind:this={svg} width="960" height="600"></svg>