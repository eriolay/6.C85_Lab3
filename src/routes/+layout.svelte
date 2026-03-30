<script>
    import { base } from "$app/paths";
    import { page } from "$app/stores";



    let pages = [
        {url: "/", title: "About"},
        {url: "/projects", title: "Projects"},
        {url: "/resume", title: "Resume"},
        {url: "/contact", title: "Contact"},
        {url: "/meta", title: "Meta"},
        {url: "https://github.com/eriolay", title: "Github"}
];

let colorScheme = "light dark";
let root = globalThis.document?.documentElement;
$: root?.style.setProperty("color-scheme", colorScheme);

// let colorScheme = "light dark"; // default colorScheme value
let localStorage = globalThis.localStorage ?? {};


if (localStorage.colorScheme) { // if localStorage has a colorScheme property
  colorScheme = localStorage.colorScheme; // override the default colorScheme
}

$: localStorage.colorScheme = colorScheme; // whenever colorScheme changes, update localStorage


</script>


<label class="color-scheme-switch">
    Theme:
    <select bind:value={ colorScheme }>
        <option value="light dark">Automatic</option>
        <option value="light">Light</option>
        <option value="dark">Dark</option>
    </select>
</label>


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



<style>

:root{
    color-scheme: dark;
}

    label.color-scheme-switch {
        position: absolute;
        top: 1rem;
        right: 5rem;
        display: inline-flex;
        gap: 4px;
        font-size: 80%;
    }
    nav {
        --border-color: oklch(50% 10% 200 / 40%);
        display: flex;
        /* border-bottom-width: 1px;
        border-bottom-style: solid;
        border-bottom-color: oklch(80% 3% 200); */
        border-bottom: 2px solid var(--border-color);
        margin-bottom: 1em;
    } 
    
    a:hover {
        border-bottom-color: var(--color-accent);
        border-bottom-width: 0.4em;
        border-bottom-style: solid;
        /* background-color: oklch(from var(--color-accent) 95% 5% h); */
        background-color: color-mix(in oklch, var(--color-accent), canvas 85%);

    }

    a.current {
        flex: 1;
        /* border-bottom-width: 0.4em;
        border-bottom-style: solid;
        border-bottom-color: oklch(80% 3% 200);    */
        border-bottom: 4px solid var(--border-color); 
        }

    
</style>


<slot />
