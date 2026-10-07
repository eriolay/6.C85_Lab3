<svelte:head>

    
</svelte:head>

<script>
    import { base } from '$app/paths';
</script>

<title>Resume</title>

  <h1>Resume</h1>
    
    
    <div class="resume-viewer">
        <iframe src={`${base}/myResume.pdf`}
        title="Eri-ife Olayinka Resume"
        ></iframe>
    </div>

    <div class="resume-actions">
        <a href={`${base}/myResume.pdf`} target="_blank" rel="noopener">
            View in New Tab
        </a>

        <a href={`${base}/myResume.pdf`} download="Eri-ife-Olayinka-Resume.pdf">
            Download Resume
        </a>
    </div>




    
    <style>
    .resume-viewer {
        width: 100%;
        height: 100vh;
        padding-bottom: 1rem;

    }
    iframe {
        width: 100%;
        height: 100%;
        border: none;
    }
    a:hover {
        border-bottom-color: var(--color-accent);
        border-bottom-width: 0.4em;
        border-bottom-style: solid;
        /* background-color: oklch(from var(--color-accent) 95% 5% h); */
        background-color: color-mix(in oklch, var(--color-accent), canvas 85%);

    }
    .resume-actions {
        display: flex;
        gap: 1rem;
    }

    .resume-actions a {
        padding: 0.7rem 1.2rem;
        border: 1px solid currentColor;
        border-radius: 6px;
        text-decoration: none;
    }
</style>