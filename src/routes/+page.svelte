<script>
    import mapboxgl from "mapbox-gl";
    import "../../node_modules/mapbox-gl/dist/mapbox-gl.css" 
    import { onMount } from "svelte";

    mapboxgl.accessToken = "pk.eyJ1Ijoib2xpdmlhOTg3IiwiYSI6ImNsdXVzZ3R6bTBkY2wyaW5reG0zYXp2ZmQifQ.7knWre2RB222IHPl2qrgjg";

    onMount( () => {
        let map = new mapboxgl.Map({
            container: 'map',
            style: 'mapbox://styles/olivia987/cluuu60oe00q101qs4jrxedq9',
            center: [ -71.0961891, 42.3591965 ],
            zoom: 12,
            // minZoom: 5,
            // maxZoom:20,
        });
    });
</script>

<style>
    @import url("$lib/global.css");

    #map{
        flex:1;
        /* background-color: black; */
    }
</style>

<h1>Bike Watching</h1>
<div id='map'></div>
