<script>

	import { onMount } from 'svelte';
	import maplibregl from 'maplibre-gl';
	import Select from "svelte-select";
	import cma2021dbf from '../data/cma-2021-dbf.geo.json';
	import cmaSummary from "../data/cma-summary.json";
	import torontoPM2021 from '../data/2021ToPM25.geo.json';
	import hamiltonPM2021 from '../data/2021HmtnPM25.geo.json';
	import calgaryPM2021 from '../data/2021CalgaryPM25.geo.json';
	import edmontonPM2021 from '../data/2021EdmnPM25.geo.json';
	import kitchCambWatPM2021 from '../data/2021KCWPM25.geo.json';
	import montrealPM2021 from '../data/2021MtlPM25.geo.json';
	import ottawaGatineauPM2021 from '../data/2021OtGaPM25.geo.json';
	import quebecPM2021 from '../data/2021QbcPM25.geo.json';
	import vancouverPM2021 from '../data/2021VcrPM25.geo.json';
	import winnipegPM2021 from '../data/2021WpgPM25.geo.json';
	

	let pageHeight;
	let pageWidth;
	let map;
    let cmaSelected = "";
    let cmauidSelected = 535;

	let mapHeight = 600;
	$: if (pageHeight < 800) {
		mapHeight = pageHeight - 200;
	} else {
		mapHeight = 600
	}

		
	// Changing the CMA - zooming to new CMA

	let filteredData;
	$: filteredData = cmaSummary.filter(
		(item) => item.cmaSummary === cmauidSelected
	)[0];
	
	//alphabetize CMAs so the desired CMA is easy to find in the dropdown
	let cmaAll = cmaSummary
		.sort( function (a, b) { 
			return a.cmaname < b.cmaname ? -1 : 1;
		})
		.map((item) => item.cmaname);

	function cmaSelectDropDown(e) {
		cmaSelectMapUpdate(e.detail.value);
	}

	function cmaSelectMapUpdate(cmaname) {
		cmaSelected = cmaname;
		let filteredData = cmaSummary.filter(
			(item) => item.cmaname === cmaSelected
		)[0];

		cmauidSelected = filteredData.cmauid;
		let cmaX = filteredData.x;
		let cmaY = filteredData.y;

		map.setZoom(9);
		map.setBearing(0);
		map.setPitch(0);
		map.panTo([cmaX, cmaY])

		const cmaFilter = [
            "match",
            ["get", "cmauid"],
            [cmauidSelected.toString()],
            true,
            false,
        ];
        map.setFilter(cma2021dbf, cmaFilter);
        //map.setFilter("metro-mindset-cma-2021-border", cmaFilter);
        //map.setFilter("metro-mindset-cma-2021-background", cmaFilter);
        //map.setFilter("municipalLabels", cmaFilter);

	}

	async function loadCensusTract(cmauid) {
        if (cmauid === cmauidSelected) {
            try {
                const response = await fetch(`/metropolitan-mindset/ct-${cmauid}-2021.topo.json`);
			}
			catch (error) {
                console.error("Error loading CT data files:", error);
            }
		}
	}

	onMount(() => {

		map = new maplibregl.Map({
			container: 'map',
			center: [-79.36, 43.715], 
			zoom: 10.5,
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

	map.addSource('torontoPM2021', {
		'type': 'geojson',
		'data': torontoPM2021
	});
	map.addLayer({
		'id': 'torontoPM2021',
		'type': 'fill',
		'source': 'torontoPM2021',
		'paint': {
			'fill-color': [
				'interpolate',
				['linear'], 
				['get', 'VALUE'],
				7.2,
				'#8DBF2E',
				9.05,
				'#F1C500',
				10.9,
				'#DC4633'
			] 
		}
	}, 'cma2021dbf');

	map.addSource('hamiltonPM2021', {
		'type': 'geojson',
		'data': hamiltonPM2021
	});
	
	map.on('load', function () {
		map.addLayer({
			'id': 'hamiltonPM2021',
			'type': 'fill',
			'source': 'hamiltonPM2021',
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
	});
	map.addSource('calgaryPM2021', {
		'type': 'geojson',
		'data': calgaryPM2021
	});
	map.addLayer({
		'id': 'calgaryPM2021',
		'type': 'fill',
		'source': 'calgaryPM2021',
		'paint': {
			'fill-color': [
				'interpolate',
				['linear'], 
				['get', 'V'],
				9.2,
				'#8DBF2E',
				12.2,
				'#F1C500',
				15.2,
				'#DC4633'
			] 
		}
	}, 'cma2021dbf');

	map.addSource('edmontonPM2021', {
		'type': 'geojson',
		'data': edmontonPM2021
	});
	map.addLayer({
		'id': 'edmontonPM2021',
		'type': 'fill',
		'source': 'edmontonPM2021',
		'paint': {
			'fill-color': [
				'interpolate',
				['linear'], 
				['get', 'V'],
				6.3,
				'#8DBF2E',
				8.75,
				'#F1C500',
				11.2,
				'#DC4633'
			] 
		}
	}, 'cma2021dbf');

	map.addSource('kitchCambWatPM2021', {
		'type': 'geojson',
		'data': kitchCambWatPM2021
	});
	map.addLayer({
		'id': 'kitchCambWatPM2021',
		'type': 'fill',
		'source': 'kitchCambWatPM2021',
		'paint': {
			'fill-color': [
				'interpolate',
				['linear'], 
				['get', 'V'],
				8,
				'#8DBF2E',
				8.65,
				'#F1C500',
				9.3,
				'#DC4633'
			] 
		}
	}, 'cma2021dbf');

	map.addSource('montrealPM2021', {
		'type': 'geojson',
		'data': montrealPM2021
	});
	map.addLayer({
		'id': 'montrealPM2021',
		'type': 'fill',
		'source': 'montrealPM2021',
		'paint': {
			'fill-color': [
				'interpolate',
				['linear'], 
				['get', 'V'],
				6.8,
				'#8DBF2E',
				8.65,
				'#F1C500',
				10.5,
				'#DC4633'
			] 
		}
	}, 'cma2021dbf');

	map.addSource('ottawaGatineauPM2021', {
		'type': 'geojson',
		'data': ottawaGatineauPM2021
	});
	map.addLayer({
		'id': 'ottawaGatineauPM2021',
		'type': 'fill',
		'source': 'ottawaGatineauPM2021',
		'paint': {
			'fill-color': [
				'interpolate',
				['linear'], 
				['get', 'V'],
				5.6,
				'#8DBF2E',
				6.65,
				'#F1C500',
				7.7,
				'#DC4633'
			] 
		}
	}, 'cma2021dbf');

	map.addSource('quebecPM2021', {
		'type': 'geojson',
		'data': quebecPM2021
	});
	map.addLayer({
		'id': 'quebecPM2021',
		'type': 'fill',
		'source': 'quebecPM2021',
		'paint': {
			'fill-color': [
				'interpolate',
				['linear'], 
				['get', 'V'],
				5.6,
				'#8DBF2E',
				7.8,
				'#F1C500',
				10,
				'#DC4633'
			] 
		}
	}, 'cma2021dbf');

	map.addSource('vancouverPM2021', {
		'type': 'geojson',
		'data': vancouverPM2021
	});
	map.addLayer({
		'id': 'vancouverPM2021',
		'type': 'fill',
		'source': 'vancouverPM2021',
		'paint': {
			'fill-color': [
				'interpolate',
				['linear'], 
				['get', 'V'],
				4.6,
				'#8DBF2E',
				5.45,
				'#F1C500',
				6.3,
				'#DC4633'
			] 
		}
	}, 'cma2021dbf');

	map.addSource('winnipegPM2021', {
		'type': 'geojson',
		'data': winnipegPM2021
	});
	map.addLayer({
		'id': 'winnipegPM2021',
		'type': 'fill',
		'source': 'winnipegPM2021',
		'paint': {
			'fill-color': [
				'interpolate',
				['linear'], 
				['get', 'V'],
				8.6,
				'#8DBF2E',
				9.35,
				'#F1C500',
				10.1,
				'#DC4633'
			] 
		}
	}, 'cma2021dbf');
});


</script>


<svelte:window bind:innerHeight={pageHeight} bind:innerWidth={pageWidth}/>

<div id="map" style="height: {mapHeight}px"></div>

<p>Data Sources: OpenStreetMap, City of Toronto, Washington University in St. Louis</p>
<!-- full reference for air pollution data: Aaron van Donkelaar, Melanie S. Hammer, Liam Bindle, Michael Brauer, Jeffery R. Brook, 
	Michael J. Garay, N. Christina Hsu, Olga V. Kalashnikova, Ralph A. Kahn, Colin Lee, Robert C. Levy, 
	Alexei Lyapustin, Andrew M. Sayer and Randall V. Martin (2021). Monthly Global Estimates of Fine Particulate 
	Matter and Their Uncertainty Environmental Science & Technology, 2021, doi:10.1021/acs.est.1c05309-->



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


<main>
	
	<div id="select-wrapper">
		<Select
			id="select"
			items={cmaAll}
			value={cmaSelected}
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

	

	 <!--<form>
		<fieldset id='cmaFilter'>
		  <select id="attribute" name="attribute">
			<option value="default">All CMAs</option>
			<option value="Toronto">Toronto</option> 
			<option value="Vancouver">Vancouver</option> 
		</fieldset>
	  </form> -->
</main>


