<script>
    import { scaleBand, scaleLinear } from "d3-scale";
    import { select, selectAll} from "d3-selection";
    import { onMount } from "svelte";

  
    export let data;

    const width = 1200;
    const height = 1000;
  
    const margin = { top: 20, right: 20, bottom: 20, left: 180 };
    const innerHeight = height - margin.top - margin.bottom;
    const innerWidth = width - margin.left - margin.right;
  
    $: xDomain = data.slice(0, 50).map((d) => d.hero1);
    $: yDomain = data.slice(0, 50).map((d) => +d.value);
   
    $: yScale = scaleBand().domain(xDomain).range([0, innerHeight]).padding(0);
    $: xScale = scaleLinear()
      .domain([0, Math.max.apply(0, yDomain)])
      .range([0, innerWidth]);
    

      onMount(() => {
        const bars = selectAll(".bar");

        bars.on("mouseover", function (_, d) {
          select(this).attr("fill", "lightgray");
        });

        bars.on("mouseout", function (_, d) {
          select(this).attr("fill", "black");
        });

        return () => {
          bars.on("mouseover", null);
          bars.on("mouseout", null);
        };
      });
  </script>
  
  <svg {width} {height}>
    <g transform={`translate(${margin.left},${margin.top})`}>
      {#each xScale.ticks() as tickValue}
        <g transform={`translate(${xScale(tickValue)},0)`}>
          <line y2={innerHeight * 2} stroke="black" />
          <text text-anchor="middle" dy=".71em" y={innerHeight + 3}>
            {tickValue}
          </text>
        </g>
      {/each}
      {#each data.slice(0, 50) as d}
        <text
          text-anchor="end"
          x="-3"
          dy=".32em"
          y={yScale(d.hero1) + yScale.bandwidth() / 2}
        >
          {d.hero1}
        </text>
        <rect
          x="0"
          y={yScale(d.hero1)}
          width={xScale(d.value)}
          height={yScale.bandwidth()/1.25}
          fill="black"
          class="bar"
        />
      {/each}
    </g>
    
  </svg>

  