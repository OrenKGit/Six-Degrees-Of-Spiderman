<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    export let graph;
  
    let svg;
    let simulation;
    let toggleState = true;
    let filteredNodes;


    $: links = graph.links.map((d) => Object.create(d));
    $: nodes = graph.nodes
        .filter((node) => node.group === 0).map((d) => {
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

    function toggle() {
        toggleState = !toggleState;
        filteredNodes = nodes.filter(node => {
            if (toggleState) {
                return node.id === "10532"; // Assuming node.id is a string
            } else {
                return node.group === 0; // Assuming 'Spiderman' corresponds to group 0
            }
        });
        update(filteredNodes);
    }

    function update(nodes) {
        svgGroup.selectAll('circle').remove();

        const maxNodeSize = d3.max(nodes, d => d.size);
        const node = svgGroup
            .selectAll('circle')
            .data(nodes)
            .enter()
            .append('circle')
            .attr('r', d => Math.sqrt(d.size / maxNodeSize) * 100)
            .style("fill", function(d) { return groupColour(this, d); })
            .call(drag(simulation));

        node.append('title')
            .text(d => d.name);

        simulation.nodes(nodes).on('tick', () => {
            node
                .attr('cx', d => d.x)
                .attr('cy', d => d.y);
        });

        simulation.alpha(1).restart(); // Restart the simulation
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
    const centers = Array.from({length: numClusters}, (_, i) => ({
        x: (i + 1) * (width / (numClusters + 1)) * spacingFactor,
        y: height / 2
    }));
    // add transition from random to grouped view
    // add legend for group categories
    onMount(async () => {

        svgGroup = d3.select(svg).append('g'); 

        d3.select(svg).call(zoom);
        update(nodes);
        
        // d3.select(svg).call(zoom.transform, d3.zoomIdentity.scale(0.35));

      const maxNodeSize = d3.max(nodes, d => d.size);
  
      simulation = d3.forceSimulation(nodes)
      .velocityDecay(0.5)
        .force('charge', d3.forceManyBody().strength(-200).distanceMin(1).distanceMax(50))
        .force('center', d3.forceCenter(width / 2, height / 2).strength(0.05))
        .force('collide', d3.forceCollide().radius(d => Math.sqrt(d.size / maxNodeSize) * 100 + 1));

        const node = svgGroup
        .selectAll('circle')
        .data(nodes)
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
    
  </script>
  <button on:click={toggle}>Spiderman</button>

  <svg bind:this={svg} width="960" height="600"></svg>