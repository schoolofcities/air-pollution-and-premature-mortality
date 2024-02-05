<script>
	import {onMount} from 'svelte';
	import maplibregl from 'maplibre-gl';
	import Select from "svelte-select";
	import cma2006 from '../data/2006cmas33.geo.json';
	import cmaSummary from "../data/cma-summary.json";

	// pmtiles style loading
	import layers from 'protomaps-themes-base';

	// let PMTILES_URL = "https://build.protomaps.com/20240123.pmtiles?key=30ce074e38619138";
	// let protocol = new pmtiles.Protocol();
	// maplibregl.addProtocol('pmtiles', protocol.tile);


	let pageHeight;
	let pageWidth;
	
	let map;

	let mapHeight = 600;
	$: if (pageHeight < 800) {
		mapHeight = pageHeight - 200;
	} else {
		mapHeight = 600
	};

	let mapWidth = 1200;
	$: if (pageWidth < 1200) {
		mapWidth = pageWidth
	} else {
		mapWidth = 1200
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

		map.setZoom(9);
		map.setBearing(0);
		map.setPitch(0);
		map.panTo([cmaX, cmaY]);

	}	

	// function for loading new vector data if the cmaname changes
	let cmaPolygon;
	async function loadCMA(cmaname) {
		try {
			const response = await fetch(`/air-pollution-and-premature-mortality/${cmaname}.geo.json`);
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
			map.removeLayer("cma2006");
			map.removeSource("cma2006");
			map.addSource('cma2006', {
				'type': 'geojson',
				'data': cma2006
			});
			map.addLayer({
				'id': 'cma2006',
				'type': 'line',
				'source': 'cma2006',
				'filter':  ['==','CMANAME', cmaname],
				'paint': {
					'line-color': '#1E3765',
					'line-width' : 2,
					'line-opacity': 1
			}
			}, "places_locality");
			map.addSource('cmaPolygon', {
				'type': 'geojson',
				'data': cmaPolygon
			});
			map.addLayer({
				'id': 'cmaPolygon',
				'type': 'fill',
				'source': 'cmaPolygon',
				'paint': {
					'fill-opacity': 1
				}
			}, 'water');
			loadPollution(checked)
		} catch {
			map.addSource('cma2006', {
				'type': 'geojson',
				'data': cma2006
			});
			map.addLayer({
				'id': 'cma2006',
				'type': 'line',
				'source': 'cma2006',
				'filter': ['==','CMANAME', cmaname],
				'paint': {
					'line-color': '#1E3765',
					'line-width' : 2,
					'line-opacity': 1
			}
			}, "places_locality");
			map.addSource('cmaPolygon', {
				'type': 'geojson',
				'data': cmaPolygon
			});
			map.addLayer({
				'id': 'cmaPolygon',
				'type': 'fill',
				'source': 'cmaPolygon',
				'paint': {
					'fill-opacity': 1
				}
			}, 'water');
			loadPollution(checked)
		}
	}

	// function for changing the type of pollution displayed
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
						3.75,
						'#6fc7ea',
						6.5,
						'#f1c500',
						8.75,
						'#e7861a',
						11,
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
							7.5,
							'#6fc7ea',
							15,
							'#f1c500',
							22.5,
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
			maxZoom: 11,
			bearing: 0,
			projection: 'globe',
			scrollZoom: true,
			attributionControl: false,
			style: {
				"version": 8,
				"name": "Empty",
				"glyphs": 
				//"https://protomaps.github.io/basemaps-assets/fonts/{fontstack}/{range}.pbf",
				"https://schoolofcities.github.io/fonts/fonts/{fontstack}/{range}.pbf",
				"sources": {
					'protomaps': {
						type: 'vector',
						url: 'https://api.protomaps.com/tiles/v3.json?key=3881c303b9fe6eb9'
					}
				},
				"layers": [
					{
						"id": "bg",
						"type": "background",
						"paint": {
							"background-color": "#f5f5f5",
							"background-opacity": 1
						}
					}
				]
			}
		});

		map.dragRotate.disable();
		map.touchZoomRotate.disableRotation();
		map.scrollZoom.disable();

		// console logging what protomaps layers are available // can remove this later
		let protoLayers = layers("protomaps","white");
		console.log(protoLayers);


		// initial layers to load on the map (water, roads)
	
		map.on('load', function() {

			map.addLayer({
				"id": "water",
				"type": "fill",
				"source": "protomaps",
				"source-layer": "water",
				"paint": {
					"fill-color": "#bdbfbf",
					"fill-outline-color": "#dedede"
				}
			})

			map.addLayer({
				"id": "roads_major",
				"type": "line",
				"source": "protomaps",
				"source-layer": "roads",
				"paint": {
					"line-color": "white",
					"line-opacity": 0.65,
					"line-width": [
						"interpolate",
						[
							"exponential",
							1
						],
						[
							"zoom"
						],
						6, 0, 10, 1
					]
				}
			});

			map.addLayer({
				"id": "roads_highway",
				"type": "line",
				"source": "protomaps",
				"source-layer": "roads",
				"filter": [
					"all",
					[
						"==",
						"pmap:kind",
						"highway"
					]
				],
				"paint": {
					"line-color": "white",
					"line-opacity": 0.65,
					"line-width": [
						"interpolate",
						[
							"exponential",
							1
						],
						[
							"zoom"
						],
						6, 1, 10, 1.5
					]
				}
			
			});

			map.addLayer({
				
				"id": "places_locality",
				"type": "symbol",
				"source": "protomaps",
				"source-layer": "places",
				"filter": [
					"==",
					"pmap:kind",
					"locality"
				],
				"layout": {
					"text-field": "{name}",
					"text-font": [
					"case",
					[
						"<=",
						[
						"get",
						"pmap:min_zoom"
						],
						5
					],
					[
						"literal",
						[
						"TradeGothic LT Regular"
						]
					],
					[
						"literal",
						[
						"TradeGothic LT Regular"
						]
					]
					],
					"text-padding": [
					"interpolate",
					[
						"linear"
					],
					[
						"zoom"
					],
					5,
					3,
					8,
					7,
					12,
					11
					],
					"text-size": [
					"interpolate",
					[
						"linear"
					],
					[
						"zoom"
					],
					2,
					[
						"case",
						[
						"<",
						[
							"get",
							"pmap:population_rank"
						],
						13
						],
						8,
						[
						">=",
						[
							"get",
							"pmap:population_rank"
						],
						13
						],
						13,
						0
					],
					4,
					[
						"case",
						[
						"<",
						[
							"get",
							"pmap:population_rank"
						],
						13
						],
						10,
						[
						">=",
						[
							"get",
							"pmap:population_rank"
						],
						13
						],
						15,
						0
					],
					6,
					[
						"case",
						[
						"<",
						[
							"get",
							"pmap:population_rank"
						],
						12
						],
						11,
						[
						">=",
						[
							"get",
							"pmap:population_rank"
						],
						12
						],
						17,
						0
					],
					8,
					[
						"case",
						[
						"<",
						[
							"get",
							"pmap:population_rank"
						],
						11
						],
						11,
						[
						">=",
						[
							"get",
							"pmap:population_rank"
						],
						11
						],
						18,
						0
					],
					10,
					[
						"case",
						[
						"<",
						[
							"get",
							"pmap:population_rank"
						],
						9
						],
						12,
						[
						">=",
						[
							"get",
							"pmap:population_rank"
						],
						9
						],
						20,
						0
					],
					15,
					[
						"case",
						[
						"<",
						[
							"get",
							"pmap:population_rank"
						],
						8
						],
						12,
						[
						">=",
						[
							"get",
							"pmap:population_rank"
						],
						8
						],
						22,
						0
					]
					],
					"icon-padding": [
					"interpolate",
					[
						"linear"
					],
					[
						"zoom"
					],
					0,
					2,
					8,
					4,
					10,
					8,
					12,
					6,
					22,
					2
					],
					"text-anchor": [
					"step",
					[
						"zoom"
					],
					"left",
					8,
					"center"
					],
					"text-radial-offset": 0.2
				},
				"paint": {
					"text-color": "#4d4d4d",
					"text-halo-color": "#ffffff",
					"text-halo-width": 1
				}

			});

			layerSet(cmanameSelected);

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

	<div class = controls>
		<!-- air pollution toggle button-->
		<div class="toggle">
			<p>Air Pollution Type:</p>
			<button role="switch" aria-checked={checked} on:click={handleClick}>
					<span>PM2.5</span>
					<span>NO2</span>
				</button>
			</div>

		<div id="select-wrapper">
			<p>Census Metropolitan Area (CMA):</p>	
			<Select
				id="select"
				items={cmaAll}
				value={cmanameSelected}
				clearable={false}
				showChevron={true}
				on:input={cmaSelectDropDown}
				--background="white"
				--selected-item-color="black" 
				--height="20px"
				--width="210px"
				--item-color="black"
				--border-radius="5px"
				--border= "solid 1px #cdcdcd"
				--list-border-radius="0px"
				--font-size="14px"
				--max-height="35px"
				--item-is-active-color="#0D534D"
				--item-is-active-bg="#6FC7EA"
				/>
		</div>	
	</div>
	
	<!-- PM2.5 Legend -->
	<div id = 'pmLegend' class='legend'>
		<div class="legend-text">
			<p class="legend-text-left-align">0</p>
			<p class="legend-text-center-align">Average PM2.5 (ug/m3)</p>
			<p class="legend-text-right-align">11</p>
		</div>
		<div>
			<div class="legend-gradient" style = 'background-image: linear-gradient(to right,#1e3765,#6fc7ea); width: {mapWidth / 4 - 1}px'></div><div class="legend-gradient" style = 'background-image: linear-gradient(to right,#6fc7ea, #f1c500); width: {mapWidth / 4 - 1}px'></div><div class="legend-gradient" style = 'background-image: linear-gradient(to right,#f1c500, #e7861a); width: {mapWidth / 4 - 1}px'></div><div class="legend-gradient" style = 'background-image: linear-gradient(to right,#e7861a,#DC4633); width: {mapWidth / 4 - 1}px'></div>
		</div>
	</div>

	<!-- NO2 Legend -->
	<div id = 'noLegend' class = 'legend'>
		<div class="legend-text">
			<p class="legend-text-left-align">0</p>
			<p class="legend-text-center-align">Average NO2 (ppb)</p>
			<p class="legend-text-right-align">30</p>
		</div>
		<div>
			<div class="legend-gradient" style = 'background-image: linear-gradient(to right,#1e3765,#6fc7ea); width: {mapWidth / 4 - 1}px'></div><div class="legend-gradient" style = 'background-image: linear-gradient(to right,#6fc7ea, #f1c500); width: {mapWidth / 4 - 1}px'></div><div class="legend-gradient" style = 'background-image: linear-gradient(to right,#f1c500, #e7861a); width: {mapWidth / 4 - 1}px'></div><div class="legend-gradient" style = 'background-image: linear-gradient(to right,#e7861a,#DC4633); width: {mapWidth / 4 - 1}px'></div>
		</div>
	</div>



		<div class = 'container'>
			<div id="map" class="map" style="height: {mapHeight}px">	
				<!-- zoom buttons-->
				<div class="map-zoom-wrapper">	
					<div on:click={zoomIn} class="map-zoom">+</div>
					<div on:click={zoomOut} class="map-zoom">‒</div>	
				</div>
			</div>
		</div>
		


<style>
	#map {
		width: 100%;
		height: 100%;
		max-width: 1200px;
		margin: 0 auto;
		margin-top: 5px;
		border-top: 1px solid var(--brandGray);
		border-bottom: 1px solid var(--brandGray);
		background-color: white;
		z-index: 1;
		position:relative;
	}

	#map:hover {
		cursor: move;
	}
	
	.container {
		position: relative;
	}
	
	p {
		font-family: RobotoBold;
		margin: 0 auto;
		text-align: left;
		font-size: 14px;
		color: var(--brandBlack);
	}

	.controls {
		/* background-color: #ccc; */
		margin: 0 auto;
		width: 350px;
		font-size: 12px;
		/* position: relative; */
		margin-top: -0px;
		display: flex;
		align-items: flex-end;
	}

 	/* Toggle switch styling */
 	:root {
		--accent-color: var(--brandDarkBlue);
	}

	.toggle button:hover {
        cursor: pointer;
		background-color:lightgray;
    }

	.toggle button {
        padding: 0px;
		margin-top: 0px;
		height: 35px;
		min-width: 110px;
        background-color: #fff;
        border: 1px solid #cdcdcd;
		border-radius: 5px;
    }
   
    .toggle button span {
        pointer-events:none;
        padding: 6px;
		padding-bottom: 10px;
    }


    /* .toggle button:focus {
		outline: var(--accent-color) solid 1px;
    } */

	[role='switch'][aria-checked='false'] :first-child {
        background: var(--accent-color);
        display: inline-block;
		color: #fff;
		border-radius: 5px;
		height: 12px;
		min-width: 40px;
    }

    [role='switch'][aria-checked='true'] :last-child {
        background: var(--accent-color);
        display: inline-block;
		color: #fff;
		border-radius: 5px;
		height: 12px;
		min-width: 40px;
    }

    /* .toggle button:focus {
        box-shadow: 0 0px 8px var(--accent-color);
        border-radius: 0.1em;
    } */

	/* drop down menu for cities */
	#select-wrapper {
		z-index: 5;
		margin-top: 10px;
		margin-left: 15px;
	}

	#select-wrapper:hover {
        cursor: pointer;
    }


	/*legend background blocks*/
	.legend {
		width: 100%;
		max-width: 1200px;
		/* background-color: rgb(237, 237, 237);		 */
		margin: 0 auto;
		/* position: relative; */
		font-size: 12px;
		/* margin-top: 10px;
		margin-left: 8.5%; */
		text-align: left;
		padding-left: 1px;
		padding-top: 10px;
	}

	.legend-text {
		display: flex;
		justify-content: space-between;
	}

	.legend-text p {
		margin: 0; /* Remove default margin on paragraphs */
		margin-bottom: -2px;
		font-family: RobotoRegular;
	}

	.legend-text-left-align {
		text-align: left;
		padding-left: 1px;
	}

	.legend-text-center-align {
		text-align: center;
	}

	.legend-text-right-align {
		text-align: right;
		padding-right: 2px;
	}

	.legend-gradient {
		display: inline-block; 
		margin: 0px;
		padding: 0px;
		height: 15px;
		border-right: solid 1px white;
	}

	/* map zoom controls */
	.map-zoom-wrapper {
		margin-top: 5px;
		margin-left: 5px;
		right: 5px;
		position: absolute;
		z-index: 2;
	}

	.map-zoom {
		/* display: block; */
		/* position: relative; */
		padding: 0px;
		padding-bottom: 3px;
		padding-left: 1px;
		margin: 0px;
		font-size: 23px;
		width: 25px;
		height: 25px;
		overflow: hidden;
		overflow-y: hidden;
		background-color: var(--brandGray70);
		color: white;
		border: solid 1px var(--brandWhite);
		text-align: center;
		margin: 0 auto;
		z-index: 2;
	}
	.map-zoom:hover {
		cursor: pointer;
		background-color: var(--brandGray90);
	}
	/* style class for data sources */
	.sources {
		max-width: 1200px;
		margin: 0 auto;
		margin-bottom: 20px;
		padding-top: 3px;
		color: var(--gray);
		
	}
	.sources p {
		font-family: RobotoRegular;
		line-height: 16px;
	}

</style>




