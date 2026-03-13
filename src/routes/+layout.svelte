<script>
    import { base } from "$app/paths";
    import { page } from "$app/stores";



    let pages = [
        {url: "/", title: "About"},
        {url: "/projects", title: "Projects"},
        {url: "/resume", title: "Resume"},
        {url: "/contact", title: "Contact"},
        {url: "https://github.com/eriolay", title: "Github"}
];
</script>




<nav>
    <ul>
    {#each pages as p}
        <li>
             <a href={!p.url.startsWith("http") ? base + p.url : p.url}


                class:current={p.url === "/" // is this link the home page?
                ? $page.url.pathname === (base + "/") // if yes - set current = true if the path name matches. Else, set current = true if the path name starts correctly
                : $page.url.pathname.startsWith(base + p.url)}


                target={p.url.startsWith("http") ? "_blank" : null}>

            {p.title}

            </a>
        </li>
    {/each}
    </ul>
</nav>

<!-- 
<nav>
        <ul>
            <li><a href=".">Home</a></li>
            <li><a class="current" href="projects">Projects</a></li>
            <li><a href="contact">Contact</a></li>
            <li><a href="resume">My Resume</a></li>
            <li><a href="https://github.com/eriolay" target="_blank">My GitHub</a></li>
        </ul>
    </nav> -->

<slot />
