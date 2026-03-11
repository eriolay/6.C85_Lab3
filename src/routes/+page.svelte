 <script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";
  import readings from "$lib/reading.json";
  import ReadingItem from "$lib/ReadingItem.svelte";

</script>
<svelte:head>
  
</svelte:head>


<h1>Eri-ife Olayinka</h1>
     

    <p>Hello hello! I am here and this is my homepage for my portfolio. This is actually my second time attempting to design a portfolio for myself so let's hope it acc sticks this time lol.
        I'm so curious about how this will turn out and how much this class will actually teach me about styling webpages. I wonder if I'll walk away from this class feeling confident in my ability to design a dynamic and intresting webpage. 
        I love seeing people's portfolios that really reflect them and their work and I hope to be able to do the same with mine hehe.
    </p>
    <img src="images/headshot.jpg" alt="Me against a background of red bleachers"
    style="width:600px;height:400px;">

    <h2>Most Recent Projects</h2>
    <div class="projects-highlights">
        {#each projects.slice(0,3) as p}
        <Project data={p} />
        {/each}
    </div>

    <h2>My {readings.length} Most Recent Books</h2>
    <div class="readings">
        {#each readings as r}
        <ReadingItem data={r} />
        {/each}
    </div>

<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>
