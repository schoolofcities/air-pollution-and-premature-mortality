<script>

import Select from "svelte-select";
import {scaleLinear} from "d3";
import { tweened } from 'svelte/motion';
import { cubicOut } from 'svelte/easing';
import livesSaved from "../data/lives-saved-totals.json";


let width;

// options for dropdowns / selections
const sexAll = ["All", "Female", "Male"];
const pollutionAll = ["PM2.5 and NO2", "PM2.5", "NO2"];

// initial selected variable
let sexSelected = "All";
let pollutionSelected = "PM2.5 and NO2"

// update variables when selected
function selectSexValue(e) {
	sexSelected = e.detail.value;
}
function selectPollutionValue(e) {
	pollutionSelected = e.detail.value;
}

// updating the data for charting:
let data = livesSaved;
function updateStats(sex, pollution) {
	data = livesSaved;
	data = data.filter(d => d.Pollutant === pollution);	
}
$: updateStats(sexSelected, pollutionSelected);


$: xScale = scaleLinear()
	.domain([0, 6545])
	.range([0, width - 100])


// setting tween animation for each bar (should be more efficient way, do all at once, but not sure how at the moment)

let tweenedWidth1 = tweened(0, {
	duration: 300,
	easing: cubicOut
});

$: tweenedWidth1.set(xScale(data[1][sexSelected]));

let tweenedWidth2 = tweened(0, {
	duration: 300,
	easing: cubicOut
});

$: tweenedWidth2.set(xScale(data[2][sexSelected]));

let tweenedWidth3 = tweened(0, {
	duration: 300,
	easing: cubicOut
});

$: tweenedWidth3.set(xScale(data[3][sexSelected]));

let tweenedWidth4 = tweened(0, {
	duration: 300,
	easing: cubicOut
});

$: tweenedWidth4.set(xScale(data[4][sexSelected]));




</script>



<div class="chart-wrapper" bind:offsetWidth={width}>

	<p id="chart-title">
		Summed for 31 Census Metropolitan Areas (CMAs) in Canada
	</p>

	<div style="border-left: solid 5px #F1C500; padding-left: 10px;">
		<p>
			Lives saved by reducing air pollutant levels to air quality standard targets
		</p>
		<svg width="100%" height="30" xmlns="http://www.w3.org/2000/svg">
			<rect y=1 width="{xScale(99999)}" height="30" style="fill:#D0D1C9; opacity: 0.1"/>
			<rect y=8 width="{$tweenedWidth1}" height="15" style="fill:#00A189" />
			<text class="scenario-saved" x="{$tweenedWidth1 + 4}" y="26">{data[1][sexSelected]}</text>
		</svg>
	</div>
	
	<div style="border-left: solid 5px #6FC7EA; padding-left: 10px;">
		<p>
			Lives saved with a 10% reduction + capping at the ambient air quality standards
		</p>
		<svg width="100%" height="30" xmlns="http://www.w3.org/2000/svg">
			<rect y=1 width="{xScale(99999)}" height="30" style="fill:#D0D1C9; opacity: 0.1"/>
			<rect y=8 width="{$tweenedWidth2}" height="15" style="fill:#00A189"/>
			<text class="scenario-saved" x="{$tweenedWidth2 + 4}" y="26">{data[2][sexSelected].toLocaleString()}</text>
		</svg>
	</div>

	<div style="border-left: solid 5px #007FA3; padding-left: 10px;">
		<p>
			Lives saved with a 25% reduction + capping at the ambient air quality standards
		</p>
		<svg width="100%" height="30" xmlns="http://www.w3.org/2000/svg">
			<rect y=1 width="{xScale(99999)}" height="30" style="fill:#D0D1C9; opacity: 0.1"/>
			<rect y=8 width="{$tweenedWidth3}" height="15" style="fill:#00A189"/>
			<text class="scenario-saved" x="{$tweenedWidth3 + 4}" y="26">{data[3][sexSelected].toLocaleString()}</text>
		</svg>
	</div>

	<div style="border-left: solid 5px #1E3765; padding-left: 10px;">
		<p>
			Lives saved with a 50% reduction + capping at the ambient air quality standards
		</p>
		<svg width="100%" height="30" xmlns="http://www.w3.org/2000/svg">
			<rect y=1 width="{xScale(99999)}" height="30" style="fill:#D0D1C9; opacity: 0.1"/>
			<rect y=8 width="{$tweenedWidth4}" height="15" style="fill:#00A189"/>
			<text class="scenario-saved" x="{$tweenedWidth4 + 4}" y="26">{data[4][sexSelected].toLocaleString()}</text>
		</svg>
	</div>

</div>


<div class="select-inputs">

	<div class="select-wrapper">
		<p>Select Air Pollutants</p>
		<Select
			id="select"
			items={pollutionAll}
			value={pollutionSelected}
			clearable={false}
			searchable={false}
			showChevron={true}
			on:input={selectPollutionValue}
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

	<div class="select-wrapper">
		<p>Select Sex</p>	
		<Select
			id="select"
			items={sexAll}
			value={sexSelected}
			clearable={false}
			searchable={false}
			showChevron={true}
			on:input={selectSexValue}
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

	<svg width="100%" height="30" xmlns="http://www.w3.org/2000/svg">
		<rect y="15" width="100%" height="1" style="fill:lightgrey"/>
	</svg>

</div>



<style>

	.select-wrapper {
		max-width: 680px;
		margin-bottom: 0px;
	}

	.select-wrapper p {
		font-family: RobotoBold;
		font-size: 16px;
		margin: 0px;
		margin-top: 5px;
	}

	.select-inputs {
		margin: 0 auto;
		width: 100%;
	}
	@media screen and (min-width: 500px) {
		.select-inputs {
			margin: 0 auto;
			width: 100%;
			display: flex;
			flex-wrap: wrap;
			justify-content: left;
			float: left;
		}
		.select-wrapper {
			width: 220px;
		}
	}

	.chart-wrapper {
		margin-top: -5px;
		width: 100%;
	}

	.chart-wrapper p {
		font-family: RobotoRegular;
		font-size: 16px;
		line-height: 20px;
		padding: 0px;
		margin: 0px;
		margin-top: 10px;
		margin-bottom: 5px;
	}

	#chart-title {
		font-family: RobotoRegular;
		font-size: 14px;
		color: var(--brandBlack);
		vertical-align: bottom;
		margin-top: -18px;
		margin-bottom: 20px;
	}


	.scenario-saved {
		font-family: RobotoBold;
		font-size: 28px;
		fill: var(--brandMedGreen);
	}

</style>