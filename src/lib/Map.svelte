<script lang="ts">
    import maplibregl, { BoxZoomHandler, MouseRotateHandler } from 'maplibre-gl'
    import { onMount } from 'svelte';

    export let baseMapDate: string;
    export let satellitePictureSwitch: boolean;

    let map: maplibregl.Map;

    function addTerralLayer() {
        map.addSource('terra-source', {
            'type': 'raster',
            'tiles': [
                `https://gibs.earthdata.nasa.gov/wmts/epsg3857/best/MODIS_Terra_CorrectedReflectance_TrueColor/default/${baseMapDate}/GoogleMapsCompatible_Level9/{z}/{y}/{x}.jpg`
            ],
            'tileSize': 256
        })
        .addLayer({
            'id': 'terra',
            'type': 'raster',
            'source': 'terra-source',
            'paint': {}
        }, 'water_intermittent');
    }

    onMount(() => {
        map = new maplibregl.Map({
            container: 'map',
            style: 'https://api.maptiler.com/maps/5a2e698d-bf96-462a-a122-fbd86ebc7aae/style.json?key=r9k1CXRP7YCqI9zWOIjp',
            zoom: 0
        })
        map.on('load', () => {
            addTerralLayer();
        });
        console.log(satellitePictureSwitch);
    });

    function changeBaseMapDate() {
        if (map && baseMapDate) {
            map.removeLayer('terra');
            map.removeSource('terra-source');
            addTerralLayer();
            map.resize();
        }
    }

    function changeSatelliteToggler() {
        console.log(satellitePictureSwitch);

        if (satellitePictureSwitch && map) {
            addTerralLayer();
            map.resize();
        }
        if (!satellitePictureSwitch && map)
        {
            map.removeLayer('terra');
            map.removeSource('terra-source');
            map.resize();
        }
    }

    $: baseMapDate, changeBaseMapDate();
    $: satellitePictureSwitch, changeSatelliteToggler();
</script>

<div id="map">

</div>

<style>
    #map {
        position: absolute;
        left: 0;
        top: 0;
        bottom: 0;
        right: 0;
    }
</style>