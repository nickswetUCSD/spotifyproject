<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    import Graph from '../components/Graph.svelte';

    export let sliderYear = 'all years.';

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

<body>
    <h1 class = "graphtitle"> Spotify Billboard Hits: <c style="color:#20d45c">What's Changing? </c>
    </h1>
      <p class = "caption">Adjust the slider to see Billboard Hits by year. Data Source: <a href="https://www.datacamp.com/workspace/datasets/dataset-python-spotify-music">  <u>DataCamp</u></a>  </p>
  
      <input class='slider' bind:value = {sliderYear} type="range" min="2010" max="2019" step="1" >
      <p class = "caption" pos="r"> Showing Billboard Hits from {sliderYear} </p>
      <Graph {data} bind:sliderYear={sliderYear}/>
  </body>

  <style>

    @font-face {
      font-family: "Spotify Circular Black";
      src: url('../components/circular-spotify-text-font/CircularSpotifyText-Black.otf') format('opentype'),
    }
    @font-face {
      font-family: "Spotify Circular Medium";
      src: url('../components/circular-spotify-text-font/CircularSpotifyText-Medium.otf') format('opentype'),
    }
    
    .graphtitle {
        color: #FAFDF6;
        background-color: #2d2a32;
        font-family: 'Spotify Circular Black';
        font-size: 300%;
        padding-left: 4px;
        padding-left: 4px;
        /* border-radius: 4px;
        border-color: #20d45c;
        border-width: 10px;
        border-style:solid; */
    }
    .caption {
      color:#FAFDF6;
      background-color: #2d2a32;
      font-family: 'Spotify Circular Medium';
      left: 140px;
      top: 100px;
      position: absolute;
    }
    
    body {
        background-color: #2d2a32;
        overflow:auto;
    }
    .slider {
      background-color: #20d45c;
      -webkit-appearance: none;
      width: 400px;
      appearance: none; 
      cursor: pointer;
      outline: none;
      overflow: hidden;
      border-radius: 16px;
      left: 750px;
      top: 150px;
      position: absolute;
    }
    .slider::-webkit-slider-runnable-track {
      height: 20px;
      background: #fafdf6;
      border-radius: 16px;
    }
    .slider::-webkit-slider-thumb {
      -webkit-appearance: none;
      appearance: none; 
      height: 20px;
      width: 20px;
      background-color: #fff;
      border-radius: 50%;
      box-shadow: -407px 0 0 400px #20d45c;
      border: 2px solid #20d45c; 
      
    }
    a {
      color: #fafdf6;
    }
    a:hover {
      color: #20d45c;
    }
    .caption[pos='r'] {
      left: 820px;
      top: 100px;
      position: absolute;
    }
    </style>
