<script>
    import App from '../components/App.svelte';

    let sliderYear = 'all years.';   
    
    export function giveSliderYear() {
      return sliderYear;
    };

</script>

<body>
  <h1 class = "graphtitle"> Spotify Billboard Hits: <a style="color:#20d45c">What's Changing? </a>
  </h1>
    <p class = "caption">Adjust the slider to see Billboard Hits by year. Data Source: <l href="https://www.datacamp.com/workspace/datasets/dataset-python-spotify-music">  <u>DataCamp</u></l>  </p>

    <input class='slider' bind:value = {sliderYear} type="range" min="2010" max="2019" step="1" >
    <p class = "caption" pos="r"> Showing Billboard Hits from {sliderYear} </p>
    <App />
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
l {
  color: #fafdf6;
}
l:hover {
  color: #20d45c;
}
.caption[pos='r'] {
  left: 820px;
  top: 100px;
  position: absolute;
}
</style>