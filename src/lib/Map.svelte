<script>
	import {onMount} from 'svelte';
	import maplibregl from 'maplibre-gl';
	import Select from "svelte-select";
	import cma2006 from '../data/2006cmas33.geo.json';
	import cmaSummary from "../data/cma-summary.json";

	let pageHeight;
	let pageWidth;
	let map;

	let mapHeight = 600;
	$: if (pageHeight < 800) {
		mapHeight = pageHeight - 200;
	} else {
		mapHeight = 600
	};
 
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
			loadPollution(checked)
		} catch {
			map.addSource('cmaPolygon', {
				'type': 'geojson',
				'data': cmaPolygon
			});
			map.addLayer({
				'id': 'cmaPolygon',
				'type': 'fill',
				'source': 'cmaPolygon',
			}, 'cma2006')
			loadPollution(checked)
		}
	}
	

// 	// function for changing the type of pollution displayed
let checkValue = 'PM2.5';
let checked = false;
		
async function loadPollution(checked) {
	if (checked === false) {
		map.setPaintProperty( 
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
				])
			} else {
				map.setPaintProperty(
				'cmaPolygon', 'fill-color', [
					'interpolate',
						['linear'], 
						['get', 'NO'],
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
			])
	 }
}
	 function handleClick(event){
        let target = event.target

        let state = target.getAttribute('aria-checked')

        checked = state === 'true' ? false : true

        checkValue = checked === false ? 'PM2.5' : 'NO2'
	 }

	 $: {
        loadPollution(checked);
    }


	onMount(() => {

		map = new maplibregl.Map({
			container: 'map',
			center: [-79.580, 43.905],
			zoom: 8,
			minZoom: 6,
			maxZoom: 10,
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

</script>



<svelte:window bind:innerHeight={pageHeight} bind:innerWidth={pageWidth}/>

<div id="map" style="height: {mapHeight}px"></div>

<p>Data Sources: OpenStreetMap</p>


<main>
	<div class="toggle">
		<p>Select pollution type:
		</p>	
	<button
        role="switch"
        aria-checked={checked}
        on:click={handleClick}>
            <span>PM2.5</span>
            <span>NO2</span>
			</button>
	</div>

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
	<div id="map" class="map" style="height: {mapHeight}px">		
		<div class="map-zoom-wrapper">	
			<span on:click={zoomOut} class="map-zoom">–</span>	
			<span on:click={zoomIn} class="map-zoom">+</span>		
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
    /* Toggle switch styling */
	:root {
		--accent-color: CornflowerBlue;
		--gray: #ccc;
	}

    .toggle button {
        padding: 3px;
        background-color: #fff;
        border: 1px solid var(--gray);
		border-radius: 50px;
    }
   
    .toggle button span {
        pointer-events:none;
        padding: 8px;
    }

    /* .toggle button:focus {
        outline: var(--accent-color) solid 1px;
    } */

	[role='switch'][aria-checked='false'] :first-child,
    [role='switch'][aria-checked='true'] :last-child {
        background: var(--accent-color);
        display: inline-block;
		color: #fff;
		border-radius: 50px;
    }

    /* .toggle button:focus {
        box-shadow: 0 0px 8px var(--accent-color);
        border-radius: 0.1em;
    } */


/* The switch - the box around the slider 
.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

/* Hide default HTML checkbox 
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

/* The slider 
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #2196F3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}
/* Rounded sliders 
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
*/

</style>




