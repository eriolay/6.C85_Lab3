<script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";

</script>

<svelte:head>
  <title>Projects</title>

  <h1>My {projects.length} Projects</h1>
     <nav>
        <ul>
            <li><a href=".">Home</a></li>
            <li><a class="current" href="projects">Projects</a></li>
            <li><a href="contact">Contact</a></li>
            <li><a href="resume">My Resume</a></li>
            <li><a href="https://github.com/eriolay" target="_blank">My GitHub</a></li>
        </ul>
    </nav>

    
    <div class="projects">
    {#each projects as p}
        <Project data={p} />
    {/each}
        
    </div>

</svelte:head>
