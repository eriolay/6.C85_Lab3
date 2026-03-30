<script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";
  import ProjectNarrative from "$lib/ProjectNarrative.svelte";
  import { onMount } from 'svelte';
  import * as d3 from 'd3';
  import Bar from '$lib/Bar.svelte';


  let years = projects.map(proj => proj.year);
  let range = Math.max(...years) - Math.min(...years);


  let rawData = [];
  let wrangled = [];
  let wrangled2 = [];

  onMount(async () => {
      rawData = await d3.json('/lab6_example.json');
      // console.log(rawData);
      wrangled = d3.rollups(
      rawData,
      v => d3.sum(v, d => d.lines),
      d => d.language
  );
    //trying to calculate percentages
    let totalLines = d3.sum(wrangled, d => d[1]);
    wrangled2 = d3.rollups(
      rawData,
      v => Math.round(d3.sum(v, d => d.lines) / totalLines * 100),
      d => d.language
    );
  });

</script>

<svelte:head>
  
</svelte:head>

  <title>Projects</title>

  <h1>{projects.length} Projects over {range} Years</h1>

  <Bar />
  
  <section>
    <h2>Data wrangling result</h2>
    <pre>{JSON.stringify(wrangled, null, 2)}</pre>
  </section>

  <section>
    <h2>Percentage of Lines by Language</h2>
    <pre>{JSON.stringify(wrangled2, null, 2)}</pre>
  </section>



    
  <p>Scroll down to see my a timeline of my projects and how they've contributed to my professional and personal life</p>

    <ProjectNarrative />

  <p class="outro">Thanks for scrolling through my project story! Feel free to explore all of the projects at your leisure below.</p>

    
    <div class="projects">
    {#each projects as p}
        <Project data={p} />
    {/each}
        
    </div>

    <style>
      .outro {
        margin-bottom: 2em;
      }
    </style>
