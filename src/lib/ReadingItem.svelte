<script>
    export let data = {};
</script>


<article>
    <h3>{data.title}</h3>
    <p class="author">{data.author}</p>
    <img src={data.image} alt="Book cover">
    <p>{data.review}</p>
</article>


<style>

    h3, p {
        text-align: center;
    }

    img {
        max-width: 100%;
        margin: auto;
    }

    .author {
        font-style: italic;
        font-weight: bold;
    }
    
</style>