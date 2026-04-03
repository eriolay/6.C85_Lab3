<script>

import { base } from '$app/paths';
import { onMount } from 'svelte';
import * as d3 from 'd3';
import BarHorizontal from '$lib/BarHorizontal.svelte';

let locData = [];

let commits = [];
let width = 1000, height = 600;

let margin = { top: 50, right: 50, bottom: 50, left: 50 };

let usableArea = {
	top: margin.top,
	right: width - margin.right,
	bottom: height - margin.bottom,
	left: margin.left
};
usableArea.width = usableArea.right - usableArea.left;
usableArea.height = usableArea.bottom - usableArea.top;


       
$: [minDate, maxDate] = d3.extent(commits.map(d => d.date));
$: maxDatePlusOne = new Date(maxDate);
$: maxDatePlusOne.setDate(maxDatePlusOne.getDate() + 1);


$: [minLines, maxLines] = d3.extent(commits.map(d => d.totalLines));


$: xScale = d3.scaleTime()
              .domain([minDate, maxDatePlusOne])
              .range([usableArea.left, usableArea.right])
              .nice()

$: yScale = d3.scaleLinear()
              .domain([24, 0])
              .range([usableArea.bottom, usableArea.top]);

$: rScale = d3.scaleSqrt()
            .domain([minLines, maxLines])
            .range([5, 30]);

console.log(rScale);

let xAxis, yAxis, yAxisGridlines;
$: {
	d3.select(xAxis).call(d3.axisBottom(xScale));
	d3.select(yAxis).call(
        d3.axisLeft(yScale)
        .tickFormat(d => String(d % 24).padStart(2, "0") + ":00"));
    d3.select(yAxisGridlines).call(
         d3.axisLeft(yScale)
         .tickFormat("")   
         .tickSize(-usableArea.width));
    
}



onMount(async () => {
    locData = await d3.csv(`${base}/loc.csv`, row => ({
        ...row,
        line: Number(row.line),
        depth: Number(row.depth),   
        length: Number(row.length),
        date: new Date(row.date + "T00:00" + row.timezone),
        datetime: new Date(row.datetime)
    }));

    commits = d3.groups(locData, d => d.commit).map(([commit, lines]) => {
        let first = lines[0];
        let {author, date, time, timezone, datetime} = first;
        let ret = {
            id: commit,
            url: "https://github.com/vis-society/lab-7/commit/" + commit,
            author, date, time, timezone, datetime,
            hourFrac: datetime.getHours() + datetime.getMinutes() / 60,
            totalLines: lines.length,
            lines: lines
        };

        return ret;
    });

    commits = d3.sort(commits, d => -d.totalLines);

    console.log(commits);
});



// console.log("check", minRadius, maxRadius);

$: langData = d3.rollups(locData, v => v.length, d => d.type)
        .map(([type, count]) => ({ label: type, value: count }));


let hoveredIndex = -1;
$: hoveredCommit = commits[hoveredIndex] ?? hoveredCommit ?? {};

 


</script>

<svelte:head>
  
</svelte:head>

<title>Meta</title>

<h1>Meta</h1>

<h3>Commits by Time of Day</h3>
<svg viewBox="0 0 {width} {height}">
    <!-- horizontal gridlines-->
    <g class="gridlines" transform="translate({usableArea.left}, 0)" bind:this={yAxisGridlines} />

    <!--drawing x-axis-->
    <g transform="translate(0, {usableArea.bottom})" bind:this={xAxis} />

    <!--drawing y-axis-->
    <g transform="translate({usableArea.left}, 0)" bind:this={yAxis} />

    <g class="dots">
        {#each commits as commit, index }
            <circle
                on:mouseenter={evt => hoveredIndex = index}
                on:mouseleave={evt => hoveredIndex = -1}
                cx={ xScale(commit.datetime) }
                cy={ yScale(commit.hourFrac) }
                r={ rScale(commit.totalLines) }
                fill=darkgreen
            />
        {/each}
        
</svg>
<dl class="info tooltip">
    <dt>Author</dt>
    <dd>{ hoveredCommit.author }</dd>

	<dt>Commit</dt>
	<dd><a href="{ hoveredCommit.url }" target="_blank">{ hoveredCommit.id }</a></dd>

	<dt>Date</dt>
	<dd>{ hoveredCommit.datetime?.toLocaleString("en", {dateStyle: "full"}) }</dd>

    <dt>Time</dt>
    <dd>{ hoveredCommit.datetime?.toLocaleString("en-US", {hour: 'numeric', minute: 'numeric', hour12: true})}</dd>

    <dt>Lines Edited</dt>
    <dd>{ hoveredCommit.totalLines }</dd>
    
</dl>


<BarHorizontal data={langData}/>






<style>
	svg {
		overflow: visible;
	}
    .gridlines {
        stroke-opacity: .2;
        
    }
    .dots {
        fill-opacity: 70%;
    }
    circle {
        
        transition: 200ms;
        
        &:hover {
            fill:rgb(204, 145, 57);
            &:not(:hover){
            fill-opacity: 10%;
        }
        }
    }
    dl.info {
        display: grid;
        grid-template-columns: auto 1fr; /* labels | values */
        gap: 0.25em 1em; /* row gap | column gap */
        margin: 0;
    } 
    dl.info dt,
    dl.info dd {
    margin: 0; /* remove default spacing */
    }

    /* Make labels less prominent */
    dl.info dt {
    font-weight: normal;
    color: #666;
    }

    /* Make values stand out a bit more */
    dl.info dd {
        font-weight: 500;
        color: #000;
        }

    /* Tooltip fixed in top-left of viewport */
    .tooltip {
        position: fixed;
        top: 1em;
        left: 1em;
        background-color:oklch(90.6% 0.00324 15.469 / 0.8);
        box-shadow: gray 5px 5px 20px 2px;
        padding: 10px;
        }

</style>





