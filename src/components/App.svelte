<script>
    import * as d3 from 'd3';
    import { onMount } from 'svelte';
    import Graph from '../components/Graph.svelte';

    export let sliderYear = 2010;
    

    let data = [];

    // var root = document.querySelector(':root');
    // root.style.setProperty('--slider-color', sliderColor(sliderYear));

    $: sliderColor = d3
    .scaleSequential(d3.extent(data, (d) => d.year), d3.scaleSequential(d3.interpolateYlGn));


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
    });

</script>

<body>
    <h1 class = "graphtitle"> Spotify Billboard Hits: <c style="color:#20d45c">What's Changing? </c></h1>
      <p class = "caption">Adjust the slider to <green style="color:#20d45c">analyze 🔎</green> Billboard Hits by year. Data Source: <a href="https://www.datacamp.com/workspace/datasets/dataset-python-spotify-music">  <u>DataCamp</u></a>  </p>
  
      <input class='slider' color={sliderColor(sliderYear)} bind:value = {sliderYear} type="range" min="2010" max="2019" step="1">
      <p class = "caption" pos="r"> Showing Billboard Hits from {sliderYear} </p>
      <Graph {data} bind:sliderYear={sliderYear}/>

      <div class='description'>
        <p style='color:#FAFDF6'>
          Each colored-in point in the dataset corresponds to a top 100 song of that year, according to the Billboard 100 chart. Grayed-out points are songs from different years. More information can be found <a href="https://docs.google.com/document/d/1Kk-nE2rbHrdHtF7sgXLapOEpjhX2HgvcEUEqrFhUWyo/edit?usp=sharing"><d style="color:#20d45c">here.</d></a>
        </p>
      </div>
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
    @font-face {
      font-family: "Spotify Circular Light";
      src: url('../components/circular-spotify-text-font/CircularSpotifyText-Light.otf') format('opentype'),
    }
    :root {
      --slider-color: #20d45c;
    }
    .graphtitle {
        color: #FAFDF6;
        overflow-y: visible;
        font-family: 'Spotify Circular Black';
        font-size: 300%;
        padding-left: 4px;
        padding-right: 4px;
        height: 58px;
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
      top: 90px;
      position: absolute;
    }
    .caption[pos='r'] {
      left: 870px;
      top: 100px;
      position: absolute;
    }
    .description {
      background-color: #2d2a32;
      left: 800px;
      top: 180px;
      position: absolute;
      font-family: 'Spotify Circular Medium';
      font-size: 80%;
      max-width: 400px;
      
    }
    body {
        background-color: #2d2a32;
        overflow:auto;
    }
    .slider {
      background-color: #2d2a32;
      -webkit-appearance: none;
      width: 400px;
      cursor: pointer;
      appearance: none;
      border-width: 4px;
      border-color: white;
      overflow: hidden;
      border-radius: 16px;
      left: 800px;
      top: 150px;
      position: absolute;
    }
    .slider::-webkit-slider-runnable-track {
      height: 20px;
      background: #353535;
      border-radius: 16px;
    }
    .slider::-webkit-slider-thumb {
      -webkit-appearance: none;
      appearance: none; 
      height: 20px;
      width: 20px;
      background-color: #fff;
      border-radius: 50%;
      box-shadow: -407px 0 0 400px rgba(32, 212, 92, 0.2);
      border: 2px solid #2d2a32; 
      transition: 0.5s;
    }
    .slider::-webkit-slider-thumb:hover {
      -webkit-appearance: none;
      appearance: none; 
      height: 20px;
      width: 20px;
      background-color: #fff;
      border-radius: 50%;
      box-shadow: -407px 0 0 400px rgba(32, 212, 92, 0.8);
      border: 2px solid #20d45c; 
      transition: 0.5s;
    }
    a {
      color: #fafdf6;
      transition-duration: 0.25s;
    }
    a:hover {
      color: #20d45c;
      transition-duration: 0.25s;
    }
    

    </style>
