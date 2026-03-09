 <script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";

</script>
<svelte:head>
  
</svelte:head>


<h1>Eri-ife Olayinka</h1>
     <nav>
        <ul>
            <li><a class="current" href=".">Home</a></li>
            <li><a href="projects">Projects</a></li>
            <li><a href="contact">Contact</a></li>
            <li><a href="resume">My Resume</a></li>
            <li><a href="https://github.com/eriolay" target="_blank">My GitHub</a></li>
        </ul>
    </nav>
    

    <p>Hello hello! I am here and this is my homepage for my portfolio. This is actually my second time attempting to design a portfolio for myself so let's hope it acc sticks this time lol.
        I'm so curious about how this will turn out and how much this class will actually teach me about styling webpages. I wonder if I'll walk away from this class feeling confident in my ability to design a dynamic and intresting webpage. 
        I love seeing people's portfolios that really reflect them and their work and I hope to be able to do the same with mine hehe.
    </p>
    <img src="images/headshot.jpg" alt="Me against a background of red bleachers"
    style="width:600px;height:400px;">

    <h3>Most Recent Projects</h3>
    <div class="projects-highlights">
        {#each projects.slice(0,3) as p}
        <Project data={p} />
        {/each}
    </div>

<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>
