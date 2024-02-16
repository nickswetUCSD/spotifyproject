<script>
    import * as d3 from 'd3';
    
    export let data;
    export let sliderYear;

    const width = 640;
    const height = 480;
    const marginTop = 20;
    const marginRight = 20;
    const marginBottom = 0;
    const marginLeft = 0;

    let gx;
    let gy;


    $: x = d3
    .scaleLinear()
    .domain(d3.extent(data, (d) => d.percentiledB))
    .range([marginLeft, width - marginRight]);

    $: y = d3
    .scaleLinear()
    .domain(d3.extent(data, (d) => d.bpm))
    .range([height - marginBottom, marginTop]);

    $: color = d3
    .scaleSequential(d3.extent(data, (d) => d.year), d3.scaleSequential(d3.interpolateYlGn));

    let rectRanges = {'all years.': [43, 206], 2010: [43, 186], 2011: [63, 175], 2012: [77,184], 2013: [77, 201], 2014: [75, 192], 2015: [77,206], 2016: [82, 186], 2017: [73, 192], 2018: [77, 180], 2019: [91, 158]};

    $: scaledRect = d3
    .scaleLinear()
    .domain(d3.extent(data, (d) => d.bpm))
    .range([height - marginBottom, marginTop]);

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
    .call(d3.axisLeft(y).ticks(height / 80))
    .attr('color', 'white')
    .call((g) =>
      g
        .selectAll('.tick line')
        .clone()
        .attr('x1', 0)
        .attr('x2', width)
        .attr('stroke-opacity', (d) => (d === 0 ? 1 : 0.1))
        .attr('color', 'white')
    );

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

<div class='plot' style='text-align:left; position:absolute; left:-40px'>
    <svg
    width = {(width) * 1.2}
    height = {(height) * 1}
    viewBox = "0 0 {(width)*1} {(height)*1.1}"
    style = "max-width: 100%; height: auto;"
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
        x="155"
        y="520"
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

    <!-- TEMPO RANGE LABEL -->
    <g transform="rotate(90)">
        <text
        x="130"
        y={-width - 50}
        dy="0.32em"
        fill="#fafdf6"
        font-family = "Spotify Circular Medium"
        text-anchor="start"
        letter-spacing = 1px
        font-size="18"
        >
        Tempo Range (BPM)
        </text>
    </g>
    

    <!-- Point marks -->
    <g stroke="#fafdf6" stroke-opacity="1">
        {#if sliderYear == 'all years.'}
            {#each data as d, i}
                    <circle key={i} cx={x(d.percentiledB)} cy={y(d.bpm)} fill={color(d.year)} opacity=1 r="5" stroke="#000000"/>
            {/each}
        {/if}

        {#if sliderYear != 'all years.'}
            {#each data as d, i}
                    {#if d.year != sliderYear}
                        <circle key={i} cx={x(d.percentiledB)} cy={y(d.bpm)} opacity=0.05 r="4" stroke="#ffffff"/>
                    {/if}
            {/each}
            {#each data as d, i}
                    {#if d.year == sliderYear}
                        <circle key={i} cx={x(d.percentiledB)} cy={y(d.bpm)} fill={color(d.year)} opacity=1 r="7" stroke="#000000" stroke-width=2px/>
                    {/if}
            {/each}
        {/if}
      </g>

      <!-- Range Meter -->

      <g stroke="#fafdf6" stroke-opacity="0">
        <rect width=10px height={y(rectRanges[sliderYear][0]) - y(rectRanges[sliderYear][1])} transform='translate(650 {y(rectRanges[sliderYear][1])})'fill='#20d45c'> 
        </g>

    <!-- Range Background -->
      <g stroke="#fafdf6" stroke-opacity="0">
        <rect width={width + 20} height={y(rectRanges[sliderYear][0]) - y(rectRanges[sliderYear][1])} transform='translate(0 {y(rectRanges[sliderYear][1])})'fill='#20d45c' opacity=0.1> 
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

