<script>
    import { onMount } from 'svelte';
    import * as d3 from 'd3';
    export let graph;
  
    let svgContainer;
    let nodes = graph.nodes;
    let links = graph.links;
    let selectedOrder = "by group";
    let svgNode;
    let y;
    let orders;
    let label;
    let marginLeft;
    let path;
    let arc;
    let Y;
    

    function order(nodes, links) {
    const degree = d3.rollup(
      links.flatMap(({ source, target, value }) => [
        { node: source, value },
        { node: target, value }
      ]),
      (v) => d3.sum(v, ({ value }) => value),
      ({ node }) => node
    );
    const orders = new Map([
      ["by name", d3.sort(nodes, (a, b) => a.name.localeCompare(b.name)).map((d) => d.id)],
      ["by group", d3.sort(nodes, ({group}) => group, ({id}) => id).map(({id}) => id)],
      ["by degree", d3.sort(nodes, ({id}) => degree.get(id), ({id}) => id).map(({id}) => id).reverse()]
    ]);

    return {
      get: (key) => orders.get(key)
    };
  }

  
    
    
    onMount(async () => {
    
    const width = 1000;
    const step = 8; // overlapping text, add diff tooltip instead?
    const marginTop = 20;
    const marginRight = 20;
    const marginBottom = 20;
    marginLeft = 130;
    const height = (nodes.length - 1) * step + marginTop + marginBottom;
    orders = order(nodes, links);
    y = d3.scalePoint(orders.get(selectedOrder), [marginTop, height - marginBottom]);
    
    const color = d3.scaleOrdinal()
        .domain(nodes.map(d => d.group).sort(d3.ascending))
        .range(["#9b5fe0", "#16a4d8", "#60dbe8", "#8bd346", "#efdf48", "#f9a52c", "#d64e12"])
        .unknown("#aaa");

    const groups = new Map(nodes.map(d => [d.id, d.group]));


    function samegroup({ source, target }) {
        return groups.get(source) === groups.get(target) ? groups.get(source) : null;
    }

    const svg = d3.select(svgContainer).append("svg")
        .attr("width", width)
        .attr("height", height)
        .attr("viewBox", [0, 0, width, height])
        .attr("style", "max-width: 100%; height: auto;");


    Y = new Map(nodes.map(({id}) => [id, y(id)]));
    
    // Add an arc for each link.
    arc = function(d) {
        const y1 = Y.get(d.source);
        const y2 = Y.get(d.target);
        const r = Math.abs(y2 - y1) / 2;
        return `M${marginLeft},${y1}A${r},${r} 0,0,${y1 < y2 ? 1 : 0} ${marginLeft},${y2}`;
    }

    path = svg.insert("g", "*")
        .attr("fill", "none")
        .attr("stroke-opacity", 0.6)
        .attr("stroke-width", 1.5)
        .selectAll("path")
        .data(links)
        .join("path")
        .attr("stroke", d => color(samegroup(d)))
        .attr("d", arc);


    label = svg.append("g")
        .attr("font-family", "sans-serif")
        .attr("font-size", 10)
        .attr("text-anchor", "end")
        .selectAll("g")
        .data(nodes)
        .join("g")
        .attr("transform", d => `translate(${marginLeft},${Y.get(d.id)})`)
        .call(g => g.append("text")
            .attr("x", -6)
            .attr("dy", "0.35em")
            .attr("fill", d => d3.lab(color(d.group)).darker(2))
            .text(d => d.name)
            .classed("label", true)
            .style("opacity", "0"))
        .call(g => g.append("circle")
            .attr("r", 3)
            .attr("fill", d => color(d.group)));


    label.append("rect")
        .attr("fill", "none")
        .attr("width", marginLeft + 40)
        .attr("height", step)
        .attr("x", -marginLeft)
        .attr("y", -step / 2)
        .attr("fill", "none")
        .attr("pointer-events", "all")
        .on("pointerenter", (event, d) => {
            svg.classed("hover", true);
            label.classed("primary", n => n === d);
            label.classed("secondary", n => links.some(({source, target}) => (
            n.id === source && d.id == target || n.id === target && d.id === source
            )));
            path.classed("primary", l => l.source === d.id || l.target === d.id).filter(".primary").raise();
            d3.selectAll(".label").filter((n) => n === d || links.some(({source, target}) => (
                n.id === source && d.id == target || n.id === target && d.id === source
                ))).style("opacity", "1");
        })
        .on("pointerout", () => {
            svg.classed("hover", false);
            label.classed("primary", false);
            label.classed("secondary", false);
            path.classed("primary", false).order();
            d3.selectAll(".label").style("opacity", "0");
        });


    svg.append("style").text(`
        .hover text { fill: #aaa; }
        .hover g.primary text { font-weight: bold; fill: #333; }
        .hover g.secondary text { fill: #333; }
        .hover path { stroke: #aaa; }
        .hover path.primary { stroke: #333; }
    `);

    svgNode = svg.node();
    

    });


    function update() {
        y.domain(orders.get(selectedOrder));
        Y = new Map(nodes.map(({id}) => [id, y(id)]));

        arc = function(d) {
            const y1 = Y.get(d.source);
            const y2 = Y.get(d.target);
            const r = Math.abs(y2 - y1) / 2;
            return `M${marginLeft},${y1}A${r},${r} 0,0,${y1 < y2 ? 1 : 0} ${marginLeft},${y2}`;
        }

        // make transition smoother
        label.transition().duration(50)
                .attr("transform", d => `translate(${marginLeft},${y(d.id)})`);

        path.transition()
        .duration((750 + nodes.length * 20)) 
        .attrTween("d", d => () => arc(d));
    }

    function onOrderChange() {
        console.log(selectedOrder)
        update();
    }

  </script>
<label>Order
    <select bind:value={selectedOrder} on:change={onOrderChange}>
        <option value="by name">by name</option>
        <option value="by group">by group</option>
        <option value="by degree">by degree</option>
    </select>
</label>
  
<div id="my_dataviz" bind:this={svgContainer}></div>