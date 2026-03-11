 <script>
    export let data = {};
 </script>       
        

        <article>
            <h3>{data.title}</h3>
            <img src={data.image} alt="">
            <p>{data.description}</p>
        </article>

<style>

    img {
    max-width: 100%;
    margin: auto;
    }

    article {
    grid-template-rows: subgrid;
    grid-row: span 3;
    }

    h3, p {
        text-align: center;
    }

</style>