 <script>
    export let data = {};
 </script>       
        

        <article>
            <h2>{data.title}</h2>
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

    h2 {
    margin: 0;
    line-height: 1.1;
    text-wrap: balance;
    }

</style>