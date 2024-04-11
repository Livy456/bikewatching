<script>
    import mapboxgl from "mapbox-gl";
    import "../../node_modules/mapbox-gl/dist/mapbox-gl.css" 
    import { createEventDispatcher, onMount } from "svelte";
    import * as d3 from 'd3';

    let bike_styling = {line_width: 2, line_color: "green", line_opacity: 0.4}
    let bike_stations = [];
    let map;

    mapboxgl.accessToken = "pk.eyJ1Ijoib2xpdmlhOTg3IiwiYSI6ImNsdXVzZ3R6bTBkY2wyaW5reG0zYXp2ZmQifQ.7knWre2RB222IHPl2qrgjg";

    onMount( async() => {
        bike_stations = await d3.csv("bluebikes-stations.csv", row=>({
                ...row,
                Lat: Number(row.Lat),
                Long: Number(row.Long),
                total_docks: Number(row.Total_Docks),
            })
        );
        
        map = new mapboxgl.Map({
            container: 'map',
            style: 'mapbox://styles/olivia987/cluuu60oe00q101qs4jrxedq9',
            center: [ -71.0961891, 42.3591965 ],
            zoom: 12,
        });

        await new Promise(resolve => map.on("load", resolve));
        map.addSource("boston_route", {
            type: "geojson",
            data: "https://bostonopendata-boston.opendata.arcgis.com/datasets/boston::existing-bike-network-2022.geojson?outSR=%7B%22latestWkid%22%3A3857%2C%22wkid%22%3A102100%7D",
        });

        map.addSource("cambridge_route", {
            type: "geojson",
            data: "Cambridge_Bike_Lanes.geojson",
        })

        map.addLayer({
            id:'boston_bike_layer',
            type: 'line',
            source: "boston_route",
            paint: {
                'line-color': bike_styling.line_color,
                'line-width': bike_styling.line_width,
                'line-opacity': bike_styling.line_opacity,
            }
        });

        map.addLayer({
            id: "cambridge_bike_layer",
            type: "line",
            source: "cambridge_route",
            paint: {
                'line-color': bike_styling.line_color,
                'line-width': bike_styling.line_width,
                'line-opacity': bike_styling.line_opacity,
            }
        });
    });

    function getCoords (station)
    {
        let point = new mapboxgl.LngLat(+station.Long, +station.Lat);
        let {x, y} = map.project(point);

        return {cx: x, cy: y};
    }
</script>

<style>
    @import url("$lib/global.css");
    #map{
        flex: 1;
    }

    #map svg{
        position:absolute;
        z-index: 1; 
        /* z-index being 1, Puts the svg element in front of the map */
        /* to use z-index you need to you position property */
        width: 100%;
        height: 100%;
        pointer-events: none;
    }

</style>

<h1>Bike Watching</h1>
<div id='map'>
    <svg>
        {#each bike_stations as station}
            <circle 
                { ...getCoords(station) } r="5" fill="steelblue"
            />
        {/each}
    </svg>
</div>
