<script>

	import { onMount } from 'svelte';
	import maplibregl from 'maplibre-gl';
<<<<<<< Updated upstream
=======
	import Select from "svelte-select";
	import cma2006 from '../data/2006cmas33.geo.json';
	import cmaSummary from "../data/cma-summary.json";

>>>>>>> Stashed changes

	let pageHeight;
	let pageWidth;
	let map;

	let mapHeight = 600;
	$: if (pageHeight < 800) {
		mapHeight = pageHeight - 200;
	} else {
		mapHeight = 600
	}
<<<<<<< Updated upstream
=======
 
	//alphabetize CMAs so the desired CMA is easy to find in the dropdown
	let cmaAll = cmaSummary
	.sort( function (a, b) { 
		return a.cmaname < b.cmaname ? -1 : 1;
	})
	.map((item) => item.cmaname);


	// Functions for what happens when the CMA changes

	let cmanameSelected = 'Toronto'; // default to Toronto for now

	let filteredData;
	$: filteredData = cmaSummary.filter(
		(item) => item.cmaSummary === cmanameSelected
	)[0];

	function cmaSelectDropDown(e) {
		cmaSelectMapUpdate(e.detail.value);
	}

	function cmaSelectMapUpdate(cmaname) {
		cmanameSelected = cmaname;
		let filteredData = cmaSummary.filter(
			(item) => item.cmaname === cmanameSelected
		)[0];

		cmanameSelected = filteredData.cmaname;
		let cmaX = filteredData.x;
		let cmaY = filteredData.y;
		
		map.setZoom(8);
		map.setBearing(0);
		map.setPitch(0);
		map.panTo([cmaX, cmaY])
	}

	// function for loading new vector data if the cmaname changes
	let cmaPolygon;
	async function loadCMA(cmaname) {
		try {
			const response = await fetch(`/${cmaname}.geo.json`);
			cmaPolygon = await response.json();
			layerSet(cmaname); // function for updating the layer on the map
		} catch (error) {
			console.error("Error loading CMA data files:", error);
		}
	}
    
	// so the above function runs automatically if cmanameSelected changes
	$: {
        loadCMA(cmanameSelected);
    }

	// function for changing the source and layer on the map when new data is loaded
	function layerSet(cmaname) {
		try {
			map.removeLayer("cmaPolygon");
			map.removeSource("cmaPolygon");
			map.addSource('cmaPolygon', {
				'type': 'geojson',
				'data': cmaPolygon
			});
			map.addLayer({
				'id': 'cmaPolygon',
				'type': 'fill',
				'source': 'cmaPolygon',
			}, 'cma2006');
		} catch {
			map.addSource('cmaPolygon', {
				'type': 'geojson',
				'data': cmaPolygon
			});
			map.addLayer({
				'id': 'cmaPolygon',
				'type': 'fill',
				'source': 'cmaPolygon',
			}, 'cma2006');
		}
	}
>>>>>>> Stashed changes

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
		})	
		
<<<<<<< Updated upstream
	});

=======
		map.addSource('cma2006', {
			'type': 'geojson',
			'data': cma2006
		});
		map.addLayer({
			'id': 'cma2006',
			'type': 'line',
			'source': 'cma2006',
			'paint': {
				'line-color': '#1E3765',
				'line-width' : 3,
				'line-opacity': 1
		}
	});

});	

//zoom button functionality
	
function zoomIn() {
		map.zoomIn();
	}
	function zoomOut() {
		map.zoomOut();
	}
	function onClick(event) {
		if (event.target === element) {}
	}
>>>>>>> Stashed changes
</script>




<svelte:window bind:innerHeight={pageHeight} bind:innerWidth={pageWidth}/>

<div id="map" style="height: {mapHeight}px"></div>

<p>Data Sources: OpenStreetMap</p>


<<<<<<< Updated upstream
=======
<main>

	<div class = bar>
		<div id="select-wrapper">
			<Select
				id="select"
				items={cmaAll}
				value={cmanameSelected}
				clearable={false}
				showChevron={true}
				on:input={cmaSelectDropDown}
				--background="white"
				--selected-item-color="#6D247A"
				--height="22px"
				--item-color="#6D247A"
				--border-radius="0"
				--border="1px"
				--list-border-radius="0px"
				--font-size="14.45px"
				--max-height="30px"
				--item-is-active-color="#0D534D"
				--item-is-active-bg="#6FC7EA"
			/>
		</div>
        
		<form>
			<fieldset id="pollutiontype">
				<select id="attribute" name="attribute">
					<option value="PM2.5">PM2.5</option> 
					<option value="NO2">NO2</option> 
			</select>
		</fieldset>	
	</form>
	</div>		
	<div id="map" class="map" style="height: {mapHeight}px">		
		<div class="map-zoom-wrapper">	
			<span on:click={zoomOut} class="map-zoom">–</span>	
			<span on:click={zoomIn} class="map-zoom">+</span>		
		</div>		
	</div>	
	<h1 on:click|preventDefault|stopPropagation|capture|nonpassive={onClick}>Event listeners</h1>
	<div on:click|self={() => map.setPaintProperty( 
		'cmaPolygon', 'fill-color', [
			'interpolate',
					['linear'], 
					['get', 'PM'],
					1.9,
					'#1e3765',
					3.5,
					'#6fc7ea',
					6,
					'#f1c500',
					8.5,
					'#e7861a',
					10.9,
					'#DC4633'
				],
			)}>
		PM2.5
		<div on:click={() => {map.setPaintProperty(
			'cmaPolygon', 'fill-colour', [
				'interpolate'
				['linear'],
				['get','NO'],
				0,
				'#1e3765',
				5,
				'#6fc7ea',
				10,
				'#f1c500',
				20,
				'#e7861a',
				30,
				'#DC4633'
			])}}>
			NO2
		</div>
	</div>	

	
	
 	
	

</main>
>>>>>>> Stashed changes


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
<<<<<<< Updated upstream
</style>
=======
	.bar {
        height: 1px;
        width: 245px;
        background-color: var(--brandDarkBlue);
        padding: 0px;
        margin: 0px;
        margin-left: 5px;
        opacity: 1;
    }
	.map-zoom-wrapper {
		margin-top: 5px;
		margin-left: 5px;
		right: 5px;
		position: absolute;
	}
	.map-zoom {
		display: inline-block;
		font-size: 25px;
		max-width: 25px;
		min-width: 25px;
		background-color: var(--brandDarkBlue);
		color: white;
		border: solid 1px var(--brandGray90);
		text-align: center;
		margin: 0 auto;
	}
	.map-zoom:hover {
		cursor: pointer;
		background-color: var(--brandGray90);
	}
</style>




>>>>>>> Stashed changes
