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

        console.log(data[0].bpm)
    });

</script>

<main>
    <Graph {data}/>
</main>

<style>
    main {
    }
</style>
