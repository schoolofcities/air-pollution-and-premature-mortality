<script>

	import { onMount } from 'svelte';
	import maplibregl from 'maplibre-gl';
	import Select from "svelte-select";
	import cma2021dbf from '../data/cma-2021-dbf.geo.json';
	import cmaSummary from "../data/cma-summary.json";


	let pageHeight;
	let pageWidth;
	let map;
    

	let mapHeight = 600;
	$: if (pageHeight < 800) {
		mapHeight = pageHeight - 200;
	} else {
		mapHeight = 600
	}

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
			const response = await fetch(`/2021-${cmaname}.geo.json`);
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
				'paint': {
					'fill-color': [
						'interpolate',
						['linear'], 
						['get', 'V'],
						8,
						'#8DBF2E',
						9.05,
						'#F1C500',
						10.1,
						'#DC4633'
					] 
				}
			}, 'cma2021dbf');
		} catch {
			map.addSource('cmaPolygon', {
				'type': 'geojson',
				'data': cmaPolygon
			});
			map.addLayer({
				'id': 'cmaPolygon',
				'type': 'fill',
				'source': 'cmaPolygon',
				'paint': {
					'fill-color': [
						'interpolate',
						['linear'], 
						['get', 'V'],
						8,
						'#8DBF2E',
						9.05,
						'#F1C500',
						10.1,
						'#DC4633'
					] 
				}
			}, 'cma2021dbf');
		}
	}

	onMount(() => {

		map = new maplibregl.Map({
			container: 'map',
			center: [-79.56, 43.915], 
			zoom: 8,
			minZoom: 4,
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

		map.addControl(new maplibregl.NavigationControl()); //need to fix symbols

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

});	
	


</script>


<svelte:window bind:innerHeight={pageHeight} bind:innerWidth={pageWidth}/>

<div id="map" style="height: {mapHeight}px"></div>

<p>Data Sources: OpenStreetMap, City of Toronto, Washington University in St. Louis</p>
<!-- full reference for air pollution data: Aaron van Donkelaar, Melanie S. Hammer, Liam Bindle, Michael Brauer, Jeffery R. Brook, 
	Michael J. Garay, N. Christina Hsu, Olga V. Kalashnikova, Ralph A. Kahn, Colin Lee, Robert C. Levy, 
	Alexei Lyapustin, Andrew M. Sayer and Randall V. Martin (2021). Monthly Global Estimates of Fine Particulate 
	Matter and Their Uncertainty Environmental Science & Technology, 2021, doi:10.1021/acs.est.1c05309

Add hyperlink instead of full reference (DOI)
-->


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
	</div>
	

</main>


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
	.bar {
        height: 1px;
        width: 245px;
        background-color: var(--brandDarkBlue);
        padding: 0px;
        margin: 0px;
        margin-left: 5px;
        opacity: 1;
    }
</style>




