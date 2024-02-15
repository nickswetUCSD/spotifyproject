<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';

    let data = [];

    onMount(async () => {
        const res = await fetch(
          'https://dsc106.com/resources/data/temperatures.csv',
        );
        const csv = await res.text();
        await d3.csvParse(csv, (d, i, columns) => {
        for (let i = 1; i < 13; ++i) {
            // pivot longer
            data.push({
                date: new Date(Date.UTC(d.Year, i - 1, 1)),
                value: +d[columns[i]],
            });
        }
        });
        data = data;
    });

    console.log(data);


    // reader.addEventListener('load', function(e) {   
    //     let fileContents = e.target.result
    //     console.log(fileContents)
    // });
    
    // data = reader.readAsText(fileObj);
    // var data = d3.dsvFormat(",").parse("../data/spotify_top_music.csv");
    // console.log(data);

</script>


