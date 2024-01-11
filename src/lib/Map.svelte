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
				]);
			pmLegend.style.display = 'block';
			noLegend.style.display = 'none';
			} else {
				map.setPaintProperty(
				'cmaPolygon', 'fill-color', [
					'interpolate',
						['linear'], 
						['get', 'NO'],
						1,
						'#1e3765',
						10,
						'#6fc7ea',
						15,
						'#f1c500',
						20,
						'#e7861a',
						30,
						'#DC4633'
			]);
			noLegend.style.display = 'block';
			pmLegend.style.display = 'none';
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
<div id="content">
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

  <!-- PM2.5 Legend -->
    <div id='pmLegend' class='pmLegend'>
      <p>Average PM2.5 (ug/m3)</p>
        <div><span style='background-color: #1e3765'></span>0 - 1.9</div>
        <div><span style='background-color: #6fc7ea'></span>1.9 - 3.5</div>
        <div><span style='background-color: #f1c500'></span>3.5 - 6</div>
        <div><span style='background-color: #e7861a'></span>6 - 8.5</div>
        <div><span style='background-color: #DC4633'></span>8.5 - 10.9</div>
    </div>

  <!-- NO2 Legend -->
  <div id='noLegend' class='noLegend'>
	<p>Average NO2 (ppb)</p>
	  <div><span style='background-color: #1e3765'></span>0 - 1</div>
	  <div><span style='background-color: #6fc7ea'></span>1 - 10</div>
	  <div><span style='background-color: #f1c500'></span>10 - 15</div>
	  <div><span style='background-color: #e7861a'></span>15 - 20</div>
	  <div><span style='background-color: #DC4633'></span>20 - 30</div>
  </div>	

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
		/* max-width: 1200px; */
		border-top: 1px solid var(--brandBlack);
		border-bottom: 1px solid var(--brandBlack);
		background-color: white;
		z-index: 1;
		position:absolute;
	}
	#content {
        width: 275px;
		height: 300px;
        position: absolute;
        top: 5px;
        left: 5px;
        background-color: lightgrey; 
        border: solid 1px lightgrey;
        border-radius: 5px;
        z-index: 2; 
    }
	main {
		margin: auto 0px;
        padding: 0px;
		width: 100%;
		height: 100%;
        
	}
	p {
		margin: 0 auto;
		text-align: right;
		font-size: 10px;
		max-width: 1200px;
		color: var(--brandBlack);
	}

	/*formatting of legend background block*/
	.pmLegend {
		position: absolute;
		background-color: #fff;
		border-radius: 0.1vw;
		bottom: 0;
		margin-bottom: 2vw;
		box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1); /*horizontal shadow, vertical shadow, size of shadow, color*/
		font: 1vw 'Helvetica Neue', Arial, Helvetica, sans-serif;
		padding: 1vw;
		right: 1vw;
		z-index: 3;  /* Specifies stack order of elements */
	}
	
	.noLegend {
		position: absolute;
		background-color: #fff;
		border-radius: 0.1vw;
		bottom: 0;
		margin-bottom: 2vw;
		box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1); /*horizontal shadow, vertical shadow, size of shadow, color*/
		font: 1vw 'Helvetica Neue', Arial, Helvetica, sans-serif;
		padding: 1vw;
		right: 1vw;
		z-index: 3;  /* Specifies stack order of elements */
	}
	
	/* Styling of legend icons */
	.pmLegend div span {
		display: inline-block;
		border-radius: 50%; /*curve border to make a circle*/
		height: 0.6vw; /*size of circle*/
		width: 0.6vw;
		margin-right: 0.5vw; /*position of circle*/
	}

	.noLegend div span {
		display: inline-block;
		border-radius: 50%; /*curve border to make a circle*/
		height: 0.6vw; /*size of circle*/
		width: 0.6vw;
		margin-right: 0.5vw; /*position of circle*/
	}
	/* .bar {
        height: 1px;
        width: 245px;
        background-color: var(--brandDarkBlue);
        padding: 0px;
        margin: 0px;
		margin-top: 200px;
        margin-left: 5px;
        opacity: 1;
    } */
	#select-wrapper {
		width:275;
		z-index: 5;
		margin-top: 1px;
	}

	#select-wrapper:hover {
        cursor: pointer;
    }

	.map-zoom-wrapper {
		margin-top: 5px;
		margin-left: 5px;
		right: 5px;
		position: relative;
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
		margin: 0;
	}
	.map-zoom:hover {
		cursor: pointer;
		background-color: var(--brandGray90);
	}
    /* Toggle switch styling */
	:root {
		--accent-color: var(--brandDarkBlue);
		--gray: #ccc;
	}

	.toggle button:hover {
        cursor: pointer;
		background-color:lightgray;
    }

    .toggle button {
        padding: 3px;
        background-color: #fff;
        border: 1px solid var(--gray);
		border-radius: 50px;
		margin-left: 100px;
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

</style>




