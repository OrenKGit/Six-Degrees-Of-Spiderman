<script>
  
  import { onMount } from "svelte";
  import * as d3 from 'd3';
  export let graph;

  let canvas;
  let width = 500;
  let height = 600;
  let max = 1000;
  const nodeRadius = 5;
  let activeNode = false;
  const padding = { top: 20, right: 40, bottom: 40, left: 25 };

  $: xScale = d3.scaleLinear()
    .domain([0, 20])
    .range([padding.left, width - padding.right]);

  $: yScale = d3.scaleLinear()
    .domain([0, 12])
    .range([height - padding.bottom, padding.top]);

  $: xTicks = width > 180 ? [0, 4, 8, 12, 16, 20] : [0, 10, 20];

  $: yTicks = height > 180 ? [0, 2, 4, 6, 8, 10, 12] : [0, 4, 8, 12];

  $: d3yScale = d3.scaleLinear().domain([0, height]).range([height, 0]);

  $: links = graph.links.map((d) => Object.create(d));
  $: nodes = graph.nodes.map((d) => {
    d.size = Math.pow(graph.links
      .filter((link) => link.source == d.id || link.target == d.id)
      .map((link) => link.value)
      .reduce((a, b) => a + b), 2);
    if (d.id == "you") {
      max = d.size;
      d.details.messages = max;
    }
    return Object.create(d);
  });

  function groupColour(context, d) {
    let nodesize = 2 + Math.sqrt(d.size) / 5;
    
    let radgrad = context.createRadialGradient(
      d.x,
      d.y,
      nodesize / 3,
      d.x,
      d.y,
      nodesize / 2
    );
    radgrad.addColorStop(0, "#01abfc");
    radgrad.addColorStop(0.1, "#01abfc");
    radgrad.addColorStop(1, "#01abfc00");

    let radgrad2 = context.createRadialGradient(
      d.x,
      d.y,
      nodesize / 3,
      d.x,
      d.y,
      nodesize / 2
    );
    radgrad2.addColorStop(0, "#7A17F6");
    radgrad2.addColorStop(0.1, "#7A17F6");
    radgrad2.addColorStop(1, "#7A17F600");

    let radgrad3 = context.createRadialGradient(
      d.x,
      d.y,
      nodesize / 3,
      d.x,
      d.y,
      nodesize / 2
    );
    radgrad3.addColorStop(0, "#B635E3");
    radgrad3.addColorStop(0.1, "#B635E3");
    radgrad3.addColorStop(1, "#B635E300");

    let radgrad4 = context.createRadialGradient(
      d.x,
      d.y,
      nodesize / 3,
      d.x,
      d.y,
      nodesize / 2
    );
    radgrad4.addColorStop(0, "#E4158B");
    radgrad4.addColorStop(0.1, "#E4158B");
    radgrad4.addColorStop(1, "#E4158B00");

    let radgrad5 = context.createRadialGradient(
      d.x,
      d.y,
      nodesize / 3,
      d.x,
      d.y,
      nodesize / 2
    );
    radgrad5.addColorStop(0, "#F9123B");
    radgrad5.addColorStop(0.1, "#F9123B");
    radgrad5.addColorStop(1, "#F9123B00");
    /*
    let radgrad6 = context.createRadialGradient(
      d.x,
      d.y,
      nodesize / 3,
      d.x,
      d.y,
      nodesize / 2
    );
    radgrad6.addColorStop(0, "#F2999B");
    radgrad6.addColorStop(0.1, "#F2999B");
    radgrad6.addColorStop(1, "#F2999B00");

    let radgrad7 = context.createRadialGradient(
      d.x,
      d.y,
      nodesize / 3,
      d.x,
      d.y,
      nodesize / 2
    );
    radgrad7.addColorStop(0, "#E1911B");
    radgrad7.addColorStop(0.1, "#E1911B");
    radgrad7.addColorStop(1, "#E1911BB00");
    , radgrad6, radgrad7
    */
    let radgrads = [radgrad, radgrad2, radgrad3, radgrad4, radgrad5];
    return radgrads[d.group];
  }
  let showCard;
  let transform = d3.zoomIdentity;
  let simulation, context;
  let dpi = 1;
  onMount(() => {
    dpi = window.devicePixelRatio || 1;
    context = canvas.getContext("2d");
    resize();

    simulation = d3
      .forceSimulation(nodes)
      .force(
        "link",
        d3
          .forceLink(links)
          .id((d) => d.id)
          .distance(
            (d) => Math.pow(2, -d.value / 1) / 10
            //(d) => 2 + Math.sqrt(max) / 4 + 130 * Math.pow(2, -d.value / 1)
          )
      )
      .force("charge", d3.forceManyBody())
      .force("center", d3.forceCenter(width / 2, height / 2))
      .force('collision', d3.forceCollide().radius((d) => Math.sqrt(d.size)/3))
      .on("tick", simulationUpdate);

    // title
    d3.select(context.canvas).on("mousemove", (event) => {
      const d = simulation.find(
        transform.invertX(event.offsetX * dpi),
        transform.invertY(event.offsetY * dpi),
        50
      );

      if (d) activeNode = d;
      else activeNode = false;
    });

    d3.select(context.canvas).on("click", () => {
      if (activeNode) {
        showCard = JSON.parse(
          JSON.stringify({ id: activeNode.id, details: activeNode.details })
        );
      }
    });

    d3.select(canvas)
      .call(
        d3
          .drag()
          .container(canvas)
          .subject(dragsubject)
          .on("start", dragstarted)
          .on("drag", dragged)
          .on("end", dragended)
      )
      .call(
        d3
          .zoom()
          .scaleExtent([1 / 100, 8])
          .on("zoom", zoomed)
      );
  });

  function simulationUpdate() {
    context.save();
    context.clearRect(0, 0, context.canvas.width, context.canvas.height);
    context.translate(transform.x, transform.y);
    context.scale(transform.k, transform.k);

    links.forEach((d) => {
      context.beginPath();
      context.moveTo(d.source.x, d.source.y);
      context.lineTo(d.target.x, d.target.y);
      context.globalAlpha = 0.3;
      context.strokeStyle = "#999";
      context.lineWidth = Math.cbrt(d.value) / 1;
      context.stroke();
      context.globalAlpha = 1;
    });

    nodes.forEach((d, i) => {
      context.beginPath();
      context.arc(d.x, d.y, 2 + Math.sqrt(d.size) / 5, 0, 2 * Math.PI);
      context.strokeStyle = "transparent";
      context.lineWidth = 1.5;
      context.stroke();
      context.fillStyle = groupColour(context, d);
      context.fill();
      context.font = '100px serif';
      if (d.size > max / 50) {
        context.fillStyle = "white";
        d.id
          .split(/(?=[A-Z])/)
          .forEach((word, index) =>
            context.fillText(d.id, d.x - (d.id.length/2) * 50, d.y)
          );
        
      }
    });
    context.restore();
  }

  function zoomed(currentEvent) {
    transform = currentEvent.transform;
    simulationUpdate();
  }

  // Use the d3-force simulation to locate the node
  
  function dragsubject(currentEvent) {
    const node = simulation.find(
      transform.invertX(currentEvent.x * dpi),
      transform.invertY(currentEvent.y * dpi),
      50
    );
    if (node) {
      node.x = transform.applyX(node.x);
      node.y = transform.applyY(node.y);
    }
    return node;
  }

  function dragstarted(currentEvent) {
    if (!currentEvent.active) simulation.alphaTarget(0.3).restart();
    currentEvent.subject.fx = transform.invertX(currentEvent.subject.x);
    currentEvent.subject.fy = transform.invertY(currentEvent.subject.y);
  }

  function dragged(currentEvent) {
    currentEvent.subject.fx = transform.invertX(currentEvent.x);
    currentEvent.subject.fy = transform.invertY(currentEvent.y);
  }

  function dragended(currentEvent) {
    if (!currentEvent.active) simulation.alphaTarget(0);
    currentEvent.subject.fx = null;
    currentEvent.subject.fy = null;
  }
  
  function resize() {
    ({ width, height } = canvas);
  }
  function fitToContainer(node) {
    dpi = window.devicePixelRatio || 1;
    // Make it visually fill the positioned parent
    node.style.width = "100%";
    node.style.height = "100%";
    // ...then set the internal size to match
    node.width = node.offsetWidth * dpi;
    node.height = node.offsetHeight * dpi;
    width = node.offsetWidth * dpi;
    height = node.offsetHeight * dpi;
  }

</script>


  <h2 style="color:white">Marvel Comics Network Graph</h2>
<svelte:window on:resize={resize} />

<div on:resize={resize} class="container">
  {#if activeNode}
    <breadcrumb id="nodeDetails">
      <strong>{activeNode.id}</strong>
      <br />
      {#if activeNode.details}
        {#each Object.entries(activeNode.details) as detail}
          {detail[0]}:
          {detail[1]}
          <br />
        {/each}
      {/if}
    </breadcrumb>
  {/if}
  <canvas use:fitToContainer bind:this={canvas} />
</div>


 

<style>
	:global(body){background-color: #313131}
  canvas {
    float: left;
  }
  .container {
    width: 100%;
    height: 90%;
    position: relative;
  }
  #nodeDetails {
    position: absolute;
    top: 1%;
    left: 1%;
    width: unset;
		color:#666666;
  }
</style>
