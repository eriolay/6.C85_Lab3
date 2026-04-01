<script>

import { base } from '$app/paths';
import { onMount } from 'svelte';
import * as d3 from 'd3';
import BarHorizontal from '$lib/BarHorizontal.svelte';

let locData = [];

onMount(async () => {
    locData = await d3.csv(`${base}/loc.csv`, row => ({
        ...row,
        line: Number(row.line),
        length: Number(row.length),
        depth: Number(row.depth)
    }));
    // console.log(locData);
});



$: langData = d3.rollups(locData, v => v.length, d => d.type)
        .map(([type, count]) => ({ label: type, value: count }));




</script>

<svelte:head>
  
</svelte:head>

<title>Meta</title>

<h1>Meta</h1>

<BarHorizontal data={langData}/>




