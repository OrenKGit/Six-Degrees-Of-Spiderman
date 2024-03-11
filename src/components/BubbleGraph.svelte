<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    export let graph;
  
    let svg;
    let groupedView = false;
    let simulation;


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


    let svgGroup;

    const zoom = d3.zoom()
    .scaleExtent([0.1, 10]) 
    .on('zoom', (event) => {
      svgGroup.attr('transform', event.transform);
    });

    // add transition from random to grouped view
    // add legend for group categories
    onMount(async () => {

        svgGroup = d3.select(svg).append('g'); 

        d3.select(svg).call(zoom);

      const maxNodeSize = d3.max(nodes, d => d.size);
  
      simulation = d3.forceSimulation(graph.nodes)
        .velocityDecay(0.5)
        .force('charge', d3.forceManyBody().strength(-200).distanceMin(1).distanceMax(50))
        .force('center', d3.forceCenter(svg.clientWidth / 2, svg.clientHeight / 2).strength(0.05))
        .force('collide', d3.forceCollide().radius(d => Math.sqrt(d.size / maxNodeSize) * 100 + 1));
          
        const node = svgGroup
        .selectAll('circle')
        .data(graph.nodes)
        .enter()
        .append('circle')
        .attr('r', d =>  Math.sqrt(d.size / maxNodeSize) * 100)
        .style("fill", function(d) { return groupColour(this, d); })
        .call(drag(simulation));
  
      node.append('title')
        .text(d => d.name);
  
      simulation.on('tick', () => {
        node
          .attr('cx', d => d.x)
          .attr('cy', d => d.y);
      });
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

    function updateView() {
      if (!simulation) return;

      const maxNodeSize = d3.max(nodes, d => d.size);

      if (groupedView) {
        // Grouped View
        const [minGroup, maxGroup] = d3.extent(nodes, d => d.group);
        const padding = 5; // Adjust this value to change the amount of space between groups
        const groupScale = d3.scaleLinear()
          .domain([minGroup, maxGroup]) 
          .range([padding, svg.clientWidth - padding]);

        simulation.force('x', d3.forceX().strength(0.05).x(d => groupScale(d.group)));
        simulation.force('y', d3.forceY().strength(0.2).y(svg.clientHeight / 2));
        simulation.force('cluster').strength(0.7);
        simulation.force('collide').strength(0.0);
        
      } else {
        // Normal View
        const [minGroup, maxGroup] = d3.extent(nodes, d => d.group);
        const groupScale = d3.scaleLinear()
          .domain([minGroup, maxGroup]) 
          .range([0, svg.clientWidth]);

        simulation.force('x', d3.forceX().strength(0.05).x(d => groupScale(d.group)));
        simulation.force('y', d3.forceY().strength(0.2).y(svg.clientHeight / 2));
        simulation.force('collide').strength(0.0);
       
      }

      simulation.alpha(1).restart();
    }

    function toggleView() {
      groupedView = !groupedView;
      updateView();
    }

  </script>
  
  <!-- comment out and change position -->
  <!-- <button on:click={toggleView}>Cluster</button> -->

  <svg bind:this={svg} width="960" height="600"></svg>