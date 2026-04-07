<script>
    import * as d3 from 'd3';

    let width = 400;
    let height = 200;

    export let data = [];
    export let title = "";


    let margin = { top: 40, right: 50, bottom: 35, left: 80 };
    let innerWidth  = width  - margin.left - margin.right;
    let innerHeight = height - margin.top  - margin.bottom;

    $: xScale = d3.scaleLinear()
        .domain([0, d3.max(data, d => d.value) || 1])
        .range([0, innerWidth]);

    $: yScale = d3.scaleBand()
        .domain(data.map(d => d.label))
        .range([0, innerHeight])
        .padding(0.2);

    $: colorScale = d3.scaleOrdinal(d3.schemeBrBG[9])
        .domain(data.map(d => d.label));

    let xAxis, yAxis;


    $: if (xAxis && yAxis) {
    d3.select(xAxis).call(
        d3.axisBottom(xScale)
            .ticks(Math.min(d3.max(data, d => d.value), 10))
            // .tickFormat(d => Number.isInteger(d) ? d : "")
            // .tickValues(d3.range(0, d3.max(data, d => d.value) + 1, 50))
        );
    d3.select(yAxis).call(d3.axisLeft(yScale));
}

    $: maxBar = d3.greatest(data, d => d.value);



</script>

<div class="container">
    <svg viewBox="0 0 {width} {height}">
    <!-- chart title-->
        <text
        x={margin.left + innerWidth / 2}
        y={margin.top / 2}
        text-anchor="middle"
        class="chart-title">
        {title}
        </text>
        <!-- x-axis -->
        <g transform="translate({margin.left}, {margin.top + innerHeight})"
        bind:this={xAxis} />
        <!-- y-axis -->
        <g transform="translate({margin.left}, {margin.top})"
        bind:this={yAxis} />
        <g transform="translate({margin.left}, {margin.top})">
            {#each data as d}
                <rect
                    x={0}
                    y={yScale(d.label)}
                    width={xScale(d.value)}
                    height={yScale.bandwidth()}
                    fill={colorScale(d.label)}
                />
            {/each}

            {#if maxBar}
                <!-- highlight outline around the tallest bar -->
                <rect
                    x={0}
                    y={yScale(maxBar.label)}
                    width={xScale(maxBar.value)}
                    height={yScale.bandwidth()}
                    fill="none"
                    stroke="currentColor"
                    stroke-width="2"
                />
                
                <!-- leader line
                <line
                    x1={xScale(maxBar.value) / 2}
                    y1={yScale(maxBar.label) - 30}
                    x2={xScale(maxBar.value) / 2}
                    y2={yScale(maxBar.label)}
                    stroke="currentColor"
                    stroke-width="1"
                /> -->

                <!-- annotation text at end of leader line -->
                <text
                    x={xScale(maxBar.value) + 5}
                    y={yScale(maxBar.label) + 20}
                    dominant-baseline="start"
                    class="annotation">
                    Language with most lines
                </text>
            {/if}


            <!-- x-axis label -->
            <text
                x={innerWidth / 2}
                y={innerHeight + margin.bottom}
                text-anchor="middle"
                class="axis-label">
                Number of Lines
            </text>

            <!-- y-axis label -->
            <text
                x={-(innerHeight / 2)}
                y={-margin.left + 30}
                text-anchor="middle"
                transform="rotate(-90)"
                class="axis-label">
                Language
            </text>
        </g>
    </svg>
    <ul class="legend">
        {#each data as d}
            <li style="--color: {colorScale(d.label)}">
                <span class="swatch"></span>
                <b>{d.label}</b> <em>({d.value})</em>
            </li>
        {/each}
    </ul>
</div>



<style>
    svg {
    max-width: 100%;
    height: auto;
    overflow: visible;
    }

    .container {
        display: flex;
        margin-bottom: 5em;
    }

    .legend {
        display: block;
        flex: 1; 
    }

    .swatch {
        display: inline-block;
        width: 1rem;
        height: 1rem;
        margin-right: 0.5rem;
        background-color: var(--color);
    }

    li {
        display: flex;
        align-items: center;
        margin-bottom: 0.75rem;
    }

    em {
        margin-left: 0.5rem;
    }

    .chart-title {
        font-size: 1em;
        font-weight: bold;
        fill: currentColor;
    }

    .axis-label {
        font-size: 0.8em;
        fill: gray;
    }

    .annotation {
        font-size: 0.5em;
        fill: black;
        font-style: italic;
    }

</style>