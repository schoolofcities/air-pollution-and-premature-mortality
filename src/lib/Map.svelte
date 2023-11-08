<script>

	import { onMount } from 'svelte';
	import maplibregl from 'maplibre-gl';
	import cma2021dbf from '../data/cma-2021-dbf.geo.json';

	let pageHeight;
	let pageWidth;
	let map;

	let mapHeight = 600;
	$: if (pageHeight < 800) {
		mapHeight = pageHeight - 200;
	} else {
		mapHeight = 600
	}

	onMount(() => {

		map = new maplibregl.Map({
			container: 'map',
			center: [-79.36, 43.715], 
			zoom: 10.5,
			minZoom: 9,
			maxZoom: 13,
			bearing: 0,
			// maxBounds: [ 
			// 	[-80.28, 43.21], 
			// 	[-77.88, 44.91] 
			// ],
			projection: 'globe',
			scrollZoom: true,
			attributionControl: false,
		});

		map.dragRotate.disable();
		map.touchZoomRotate.disableRotation();
		// map.scrollZoom.disable();

		map.addSource('osm-raster-tiles', {
			'type': 'raster',
			'tiles': ['https://tile.openstreetmap.org/{z}/{x}/{y}.png'],
			'tileSize': 256,
			'minzoom': 0,
			'maxzoom': 19
		});
		map.addLayer({
			'id': 'osm-raster-tiles',
			'type': 'raster',
			'source': 'osm-raster-tiles',
			'paint': {
				'raster-saturation': -1,
				'raster-opacity': 0.42
			}
		});	
		
		map.addSource('cma2021dbf', {
			'type': 'geojson',
			'data': cma2021dbf
		});
		map.addLayer({
			'id': 'cma2021dbf',
			'type': 'line',
			'source': 'cma2021dbf',
			'paint': {
				'line-color': '#1E3765',
				'line-width' : 3,
				'line-opacity': 1
		}
	});

	// map.addSource('torontoPM2021', {
	// 	'type': 'tiff',
	// 	'data': torontoPM2021
	// });
	// map.addLayer({
	// 	'id': 'torontoPM2021',
	// 	'type': 'raster',
	// 	'source': 'torontoPM2021',
	// });
	
map.on("load", function () {
  map.loadImage("2021_Toronto_PM25.tif", function (error, image) {
    if (error) throw error;

    map.addImage("2021_Toronto_PM25.tif", image);

    map.addLayer({
      id: "TorontoPM25",
      type: "raster",
      source: "2021_Toronto_PM25.tif",
      paint: {
        "raster-opacity": 0.8, // Adjust opacity
      },
    });
  });
});

});


</script>


<svelte:window bind:innerHeight={pageHeight} bind:innerWidth={pageWidth}/>

<div id="map" style="height: {mapHeight}px"></div>

<p>Data Sources: OpenStreetMap</p>



<style>
	#map {
		width: 100%;
		margin: 0 auto;
		max-width: 1200px;
		border-top: 1px solid var(--brandBlack);
		border-bottom: 1px solid var(--brandBlack);
		background-color: white;
	}
	p {
		margin: 0 auto;
		text-align: right;
		font-size: 10px;
		max-width: 1200px;
		color: var(--brandBlack);
	}
</style>