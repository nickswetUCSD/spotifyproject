<script>
    import * as d3 from 'd3';
    
    export let data;

    const width = 928;
    const height = 600;
    const marginTop = 20;
    const marginRight = 30;
    const marginBottom = 30;
    const marginLeft = 40;

    $: x = d3
    .scaleUtc()
    .domain(d3.extent(data, (d) => d.dB))
    .range([marginLeft, width - marginRight]);

    let tooltipPt = null;
    function onPointerMove(event) {
    const i = bisect(data, x.invert(d3.pointer(event)[0]));
    tooltipPt = data[i];
    }

    function onPointerLeave(event) {
    tooltipPt = null;
    }

    //$: d3.select(svg)
    // .on('pointerenter pointermove', onPointerMove)
    // .on('pointerleave', onPointerLeave);

</script>

<div class='plot'>
    <svg
    {width}
    {height}
    viewBox = "0 0 {width} {height}"
    style = "max-width: 100%; height:auto"
    >

    <!-- Point marks -->
    <g stroke="#000" stroke-opacity="0.2">
        {#each data as d, i}
          <circle key={i} cx={x(d.dB)} cy={200} fill={'blue'} r="2.5" />
        {/each}
      </g>

      {#if tooltipPt}
      <g transform="translate({x(tooltipPt.dB)},{y(tooltipPt.dB)})">
        <text font-weight="bold">{tooltipPt.dB}</text>
      </g>
    {/if}
    </svg>
</div>

