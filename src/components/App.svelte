<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    import Graph from '../components/Graph.svelte';

    let data = [];

    onMount(async() => {
        const res = await fetch('spotify_top_music.csv'); 
        const csv = await res.text();
        data = await d3.csvParse(csv, d3.autoType);
        data = data;
        // Remove corrupted data outlier.
        data.splice(442, 1);

        //Generate dB percentile
        for (let i = 0; i < data.length; i++) {
            let percentile = 0;
            for (let j = 0; j < data.length; j++) {
                if (data[i].dB >= data[j].dB) {
                    percentile = percentile + 1;
                }
            }
            percentile = percentile*1.0 * 100 / data.length*1.0;
            data[i].percentiledB = percentile;
            
        }
        console.log(data)
    });

</script>

<main>
    <Graph {data}/>
</main>

<style>
    main {
    }
</style>
