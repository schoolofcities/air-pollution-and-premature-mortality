<script>

import Select from "svelte-select";
import livesSaved from "../data/lives-saved-totals.json";
import {scaleLinear} from "d3";



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

	console.log(data)
	
}

$: updateStats(sexSelected, pollutionSelected);


$: xScale = scaleLinear()
	.domain([0, 6545])
	.range([0, width - 100])


</script>





<div class="chart-wrapper" bind:offsetWidth={width}>

	

	<!-- <svg width="100%" height="12" xmlns="http://www.w3.org/2000/svg">
		<rect y="10" width="100%" height="1" style="fill:lightgrey"/>
	</svg> -->

	<!-- <p id="chart-title">
		Estimated Total Lives Saved by Scenario:
	</p> -->

	<div style="border-left: solid 5px #F1C500; padding-left: 10px;">
		<p>
			Lives saved by reducing air pollutant levels to air quality standard targets
		</p>
		<svg width="100%" height="30" xmlns="http://www.w3.org/2000/svg">
			<rect y=1 width="{xScale(99999)}" height="30" style="fill:#D0D1C9; opacity: 0.1"/>
			<rect y=8 width="{xScale(data[1][sexSelected])}" height="15" style="fill:#00A189"/>
			<text class="scenario-saved" x="{xScale(data[1][sexSelected]) + 4}" y="26">{data[1][sexSelected]}</text>
		</svg>
	</div>
	
	<div style="border-left: solid 5px #6FC7EA; padding-left: 10px;">
		<p>
			Lives saved with a 10% reduction + capping at the ambient air quality standards
		</p>
		<svg width="100%" height="30" xmlns="http://www.w3.org/2000/svg">
			<rect y=1 width="{xScale(99999)}" height="30" style="fill:#D0D1C9; opacity: 0.1"/>
			<rect y=8 width="{xScale(data[2][sexSelected])}" height="15" style="fill:#00A189"/>
			<text class="scenario-saved" x="{xScale(data[2][sexSelected]) + 4}" y="26">{data[2][sexSelected].toLocaleString()}</text>
		</svg>
	</div>

	<div style="border-left: solid 5px #007FA3; padding-left: 10px;">
		<p>
			Lives saved with a 25% reduction + capping at the ambient air quality standards
		</p>
		<svg width="100%" height="30" xmlns="http://www.w3.org/2000/svg">
			<rect y=1 width="{xScale(99999)}" height="30" style="fill:#D0D1C9; opacity: 0.1"/>
			<rect y=8 width="{xScale(data[3][sexSelected])}" height="15" style="fill:#00A189"/>
			<text class="scenario-saved" x="{xScale(data[3][sexSelected]) + 4}" y="26">{data[3][sexSelected].toLocaleString()}</text>
		</svg>
	</div>

	<div style="border-left: solid 5px #1E3765; padding-left: 10px;">
		<p>
			Lives saved with a 50% reduction + capping at the ambient air quality standards
		</p>
		<svg width="100%" height="30" xmlns="http://www.w3.org/2000/svg">
			<rect y=1 width="{xScale(99999)}" height="30" style="fill:#D0D1C9; opacity: 0.1"/>
			<rect y=8 width="{xScale(data[4][sexSelected])}" height="15" style="fill:#00A189"/>
			<text class="scenario-saved" x="{xScale(data[4][sexSelected]) + 4}" y="26">{data[4][sexSelected].toLocaleString()}</text>
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
		width: 100%;
		margin-bottom: 0px;
	}

	.select-wrapper p {
		font-family: RobotoBold;
		font-size: 16px;
		margin: 0px;
		margin-top: 5px;
	}

	.select-inputs {
		/* margin: 0 auto; */
		width: 100%;
	}
	@media screen and (min-width: 500px) {
		.select-inputs {
			/* margin: 0 auto; */
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

	.select-label {

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
		font-family: RobotoBold;
		font-size: 18px;
		color: var(--brandBlack);
		vertical-align: bottom;
		margin-bottom: 20px;
	}


	.scenario-saved {
		font-family: RobotoBold;
		font-size: 28px;
		fill: var(--brandMedGreen);
	}

</style>