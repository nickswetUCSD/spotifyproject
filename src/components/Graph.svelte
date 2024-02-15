<script>
    import * as d3 from 'd3';
    
    export let data;
    export let sliderYear;

    const width = 635;
    const height = 450;
    const marginTop = 20;
    const marginRight = 20;
    const marginBottom = 20;
    const marginLeft = 0;

    let gx;
    let gy;

    $: console.log(sliderYear);

    $: x = d3
    .scaleLinear()
    .domain(d3.extent(data, (d) => d.percentiledB))
    .range([marginLeft, width - marginRight]);

    $: y = d3
    .scaleLinear()
    .domain(d3.extent(data, (d) => d.bpm))
    .range([height - marginBottom, marginTop]);

    $: color = d3
    .scaleSequential(d3.extent(data, (d) => d.year), ['#000000','#20d45c']);

    //#9B7EDE indigo
    //#20d45c green
    //#fafdf6 white
    //#82735C coyote
    //#2D2A32 dark gray
    //#6184D8 cool blue

    //X AXIS AND GRIDLINES
    $: d3.select(gx).call(d3.axisBottom(x).ticks(width / 80))
    .attr('color', 'white')
    .call((g) =>
      g
        .selectAll('.tick line')
        .clone()
        .attr('y1', 0)
        .attr('y2', -height)
        .attr('stroke-opacity', (d) => (d === 0 ? 1 : 0.1))
        .attr('color', 'white')
    );
    
    //Y AXIS AND GRIDLINES
    $: d3.select(gy)
    .call(d3.axisLeft(y).ticks(height / 40))
    .attr('color', 'white')
    .call((g) =>
      g
        .selectAll('.tick line')
        .clone()
        .attr('x1', 0)
        .attr('x2', width)
        .attr('stroke-opacity', (d) => (d === 0 ? 1 : 0.1))
        .attr('color', 'white'),
    );

    let tooltipPt = null;
    function onPointerMove(event) {
    const i = bisect(data, x.invert(d3.pointer(event)[0]));
    tooltipPt = data[i];
    }

    function onPointerLeave(event) {
    tooltipPt = null;
    }

    console.log(d3.extent(data, (d) => d.percentiledB))

    //$: d3.select(svg)
    // .on('pointerenter pointermove', onPointerMove)
    // .on('pointerleave', onPointerLeave);

</script>

<div class='plot'>
    
    <svg
    {width}
    {height}
    viewBox = "0 0 {width} {height + marginBottom + marginTop}"
    style = "max-width: 100%; height:auto"
    >
    <!-- x-axis -->
    <g bind:this={gx} transform="translate({marginLeft + 40},{height - marginBottom})" />
    <!-- y-axis -->
    <g bind:this={gy} transform="translate({marginLeft + 40}, 0)">


    <!-- Y-AXIS LABEL -->
    <g transform="rotate(-90)">
        <text
        x="-330"
        y="-60"
        dy="0.32em"
        fill="#fafdf6"
        font-family = "Spotify Circular Medium"
        text-anchor="start"
        letter-spacing = 1px
        font-size="18"
        >
        Beats Per Minute (BPM)
        </text>
    </g>

    

    <!-- X-AXIS LABEL -->
    <g>
        <text
        x="140"
        y="480"
        dy="0.32em"
        fill="#fafdf6"
        font-family = "Spotify Circular Medium"
        text-anchor="start"
        letter-spacing = 1px
        font-size="18"
        >
        Relative Loudness (dB, Percentile)
        </text>
    </g>
    

    <!-- Point marks -->
    <g stroke="#fafdf6" stroke-opacity="1">
        {#each data as d, i}
            {#if sliderYear != 'all years.'}
                {#if d.year == sliderYear}
                    <circle key={i} cx={x(d.percentiledB)} cy={y(d.bpm)} fill={color(d.year)} opacity=1 r="5" />
                {/if}
            {/if}
        {/each}
      </g>

      {#if tooltipPt}
      <g transform="translate({x(tooltipPt.percentiledB)},{y(tooltipPt.percentiledB)})">
        <text font-weight="bold">{tooltipPt.percentiledB}</text>
      </g>
    {/if}
    </svg>
</div>

<style>
    svg {
        padding-left: 40px;
    }
    @font-face {
        font-family: "Spotify Circular Medium";
        src: url('../components/circular-spotify-text-font/CircularSpotifyText-Medium.otf') format('opentype'),
    }
</style>

