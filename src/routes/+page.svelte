 <script>
  import projects from "$lib/projects.json";
  import Project from "$lib/Project.svelte";
  import readings from "$lib/reading.json";
  import ReadingItem from "$lib/ReadingItem.svelte";


import { onMount } from "svelte";

let githubData = null; // This will eventually hold our Github stats
let loading = true; // This will be true *until* the fetch's promise resolves to a value
let error = null; // If the API call resulted in an error, it will go into this variable

onMount(async () => {
     try {
        console.log("Page has been mounted!")
        // let response = await fetch("https://api.github.com/users/eriolay");

        let response = await {
            ok: true,
            json: async () => ({
                followers: 100,
                following: 100,
                public_repos: 100,
                public_gists: 100,
            }),
            };

        console.log(response);
        githubData = await response.json();
        console.log(githubData);
    } catch (err) {
        error = err;
    }
    loading = false;
})


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


    {#if loading}
        <p>Loading...</p>
    {:else if error}
        <p>Something went wrong: {error.message}</p>
    {:else}
        <!-- The data is {JSON.stringify(githubData)} -->
         <section>
            <h2>My Github Stats</h2>
            <dl>
                <dt>Followers: </dt>
                <dd>{githubData.followers}</dd>
                <dt>Following: </dt>
                <dd>{githubData.following}</dd>
                <dt>Public Repos: </dt>
                <dd>{githubData.public_repos}</dd>
                <dt>Public Gists: </dt>
                <dd>{githubData.public_gists}</dd>
            </dl>
         </section>
    {/if}


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



<style>
    dl {
        display: grid;
        grid-template-columns: repeat(4, 1fr);  
    }

    dt {
        font-weight: bold;
        /* color:rgb(221, 196, 164); */
        text-transform: uppercase;
    }
</style>