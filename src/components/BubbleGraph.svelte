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

    // add transition from random to grouped view
    // add legend for group categories
    onMount(async () => {

        svgGroup = d3.select(svg).append('g'); 

        d3.select(svg).call(zoom);

      const maxNodeSize = d3.max(nodes, d => d.size);
  
      simulation = d3.forceSimulation(graph.nodes)
            .velocityDecay(0.5)
            .force('charge', d3.forceManyBody().strength(-200).distanceMin(1).distanceMax(50))
            .force('center', d3.forceCenter(width / 2, height / 2))
            .force('collide', d3.forceCollide().radius(d => Math.sqrt(d.size / maxNodeSize) * 100))
            .force('x', d3.forceX(d => xScale(d.group)).strength(1)) 
            .force('y', d3.forceY(d => yScale(d.group)).strength(1)) 

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


      const legend = svgGroup.selectAll('.legend')
            .data(radgrads)
            .enter()
            .append('g')
            .attr('class', 'legend')
            .attr('transform', (d, i) => `translate(0,${i * 20})`);

        legend.append('rect')
            .attr('x', width - 18)
            .attr('width', 18)
            .attr('height', 18)
            .style('fill', d => d);

        legend.append('text')
            .attr('x', width - 24)
            .attr('y', 9)
            .attr('dy', '.35em')
            .style('text-anchor', 'end')
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

    
  </script>

  <svg bind:this={svg} width="960" height="600"></svg>