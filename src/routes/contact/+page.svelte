<svelte:head>
  <title>Contact me</title>

  <h1>Contact me!</h1>
    <nav>
        <ul>
            <li><a href=".">Home</a></li>
            <li><a href="projects">Projects</a></li>
            <li><a class="current" href="contact">Contact</a></li>
            <li><a href="resume">My Resume</a></li>
            <li><a href="https://github.com/eriolay" target="_blank">My GitHub</a></li>
        </ul>
    </nav>
   
    <form action="mailto:126813646+eriolay@users.noreply.github.com" method="get">
            <label>Email address: 
            <input name="email" type="email" />
            </label>

            <label>Subject: 
            <input name="subject" type="text" />
            </label>

            <label>Message: 
            <textarea name="body"></textarea>
            </label>

        <button>Send</button> 
    </form>

</svelte:head>
