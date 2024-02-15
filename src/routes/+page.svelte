<script>
    import App from '../components/App.svelte';
</script>

<h1 class = "graphtitle"> Spotify Billboard Hits: What's Changing? </h1>
<p>If you are seeing this, the project is working! <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>
<p>new text emenffufuft!!!!</p>

<App />

<style>

@font-face {
  font-family: "Spotify Circular Black";
  src: url('../components/circular-spotify-text-font/CircularSpotifyText-Black.otf') format('opentype'),
}

.graphtitle {
    background-color: '#ffffff';
    font-family: 'Spotify Circular Black';
    font-size: 300%;
}
</style>