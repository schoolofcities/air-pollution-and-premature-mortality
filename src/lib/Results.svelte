<script>

import Select from "svelte-select";
import predictedDeaths from "../data/predicted-deaths-by-cma.json";
import * as d3 from "d3";

// options for dropdowns / selections
const sexAll = ["All", "Female", "Male"];
const pollutionAll = ["PM2.5 and NO2", "PM2.5", "NO2"];
const ageAll = ["All Ages", "18-24", "25-34", "35-44", "45-54", "55-64", "65-74"]
let cmaAll = [...new Set(predictedDeaths.map(item => item.CMA))].sort();

console.log(cmaAll);

// initial variables
let cmanameSelected = "Winnipeg";
let sexSelected = "All";
let pollutionSelected = "PM2.5 and NO2"
// let ageSelected = "All Ages"


// update variables when selected
function selectCmaValue(e) {
	cmanameSelected = e.detail.value;
}
function selectSexValue(e) {
	sexSelected = e.detail.value;
}
// function selectAgeValue(e) {
// 	ageSelected = e.detail.value;
// }
function selectPollutionValue(e) {
	pollutionSelected = e.detail.value;
}


let data = []

function updateStats(cma, sex, pollution, age) {

	data = predictedDeaths;

	// filter by CMA
	data = data.filter(d => d.CMA === cma);

	// filter by pollution type
	data = data.filter(d => d.Pollution === pollution);

	// filtering and grouping by age and/or sex
	if (sex === "All") {
		data = d3.rollups(
			data, 
			v => d3.sum(v, d => d["Predicted Premature Death"]), 
			d => d.Intervention, 
		);
	}
	else {
		data = data.filter(d => d.SEX === sex);
		data = d3.rollups(
			data, 
			v => d3.sum(v, d => d["Predicted Premature Death"]), 
			d => d.Intervention
		);
	}
	data = data.reduce((acc, [key, value]) => {
		acc[key] = value;
		return acc;
	}, {});
}

$: updateStats(cmanameSelected, sexSelected, pollutionSelected);

</script>



<div class="select-inputs">

	<div class="select-wrapper">
		<p>Census Metropolitan Area</p>	
		<Select
			id="select"
			items={cmaAll}
			value={cmanameSelected}
			clearable={false}
			searchable={false}
			showChevron={true}
			on:input={selectCmaValue}
			--background="white"
			--selected-item-color="black" 
			--height="20px"
			--width="220px"
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
			--width="220px"
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

	<!-- <div class="select-wrapper">
		<p>Age</p>	
		<Select
			id="select"
			items={ageAll}
			value={ageSelected}
			clearable={false}
			showChevron={true}
			on:input={selectAgeValue}
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
	</div> -->

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
			--width="220px"
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

<div class="results">

	<table>
		<tr class = column-header style = "text-align: left">
			<th>Scenario</th>
			<th class = "column-header-right">Predicted Premature<br>Deaths</th>
			<th class = "column-header-right">Estimated<br>Lives Saved</th>
		</tr>
		<tr class = deaths>
			<th></th>
			<th colspan = "2" class = "column-header" style = "font-size: 14px; font-style: italic">Per 100,000 People</th>
		</tr>
		<tr class>
			<td id = "baseline" class = "scenario-label" style="width: 430px">Existing Conditions</td>
			<td class = "scenario-baseline">{Math.floor(data.Baseline).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- premature deaths -->
		</tr>
		<tr>
			<td id = "target" class = "scenario-label" style="width: 430px">Reducing air pollutant levels to air quality standard targets</td>
			<td class = "scenario-number">{Math.floor(data.Target).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- premature deaths -->
			<td class = "scenario-saved">{(Math.floor(data.Baseline) - Math.floor(data.Target)).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- estimated lives saved -->
		</tr>
		<tr>
			<td id = "amb10" class = "scenario-label" style="width: 430px">10% reduction + capping at the ambient air quality standards</td> 
			<td class = "scenario-number">{Math.floor(data.AMB10).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- premature deaths -->
			<td class = "scenario-saved">{(Math.floor(data.Baseline) - Math.floor(data.AMB10)).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- estimated lives saved -->
		</tr>
		<tr>
			<td id = "amb25" class = "scenario-label" style="width: 430px">25% reduction + capping at the ambient air quality standards</td>
			<td class = "scenario-number"> {Math.floor(data.AMB25).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- premature deaths -->
			<td class = "scenario-saved">{(Math.floor(data.Baseline) - Math.floor(data.AMB25)).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- estimated lives saved -->
		</tr>
		<tr>
			<td id = "amb50" class = "scenario-label" style="width: 430px">50% reduction + capping at the ambient air quality standards</td>
			<td class = "scenario-number"> {Math.floor(data.AMB50).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- premature deaths -->
			<td class = "scenario-saved">{(Math.floor(data.Baseline) - Math.floor(data.AMB50)).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- estimated lives saved -->
		</tr>
	</table>
	<br>
	
</div>




<style>

p {
	font-family: RobotoBold;
	font-size: 16px;
	margin: 0px;
	margin-top: 5px;
}

.select-inputs {
	margin: 0 auto;
	width: 230px;
}

@media screen and (min-width: 480px) {
	.select-inputs {
		margin: 0 auto;
		width: 460px;
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
	}
	.select-wrapper {
		width: 50%; /* Adjust the width as needed */
  	}
	.results {
		margin: 0 auto;
		max-width: 680px;
		display: flex;
		flex-wrap: wrap;
	}
}
@media screen and (min-width: 691px) {
	.select-inputs {
		margin: 0 auto;
		width: 690px;
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
	}
  	.select-wrapper {
		width: 33%; /* Adjust the width as needed */
  	}
	.results {
		margin: 0 auto;
		max-width: 680px;
		display: flex;
		flex-wrap: wrap;
	}
}

.results {
	padding: 0px;
	margin: 0px;
}

table {
	margin-top: 20px;
	margin-left: -5px;
	border-top: solid 1px var(--brandGray);
	border-bottom: solid 1px var(--brandGray);
	border-collapse:separate;
	border-spacing: 12px 10px;
}

.column-header {
	font-family: RobotoRegular;
	font-size: 17px;
	color: var(--brandBlack);
	vertical-align: bottom;
}

.column-header-right {
	padding-left: 9px;
}

.scenario-label {
	font-family: RobotoRegular;
	font-size: 16px;
	border-top: solid 1px #f7f7f7;
}
.scenario-baseline {
	font-family: RobotoBold;
	font-size: 27px;
	color: var(--brandRed);
	border-top: solid 1px #f7f7f7;
	text-align: right;
}
.scenario-number {
	font-family: RobotoBold;
	font-size: 27px;
	color: var(--brandGray70);
	border-top: solid 1px #f7f7f7;
	text-align: right;
}
.scenario-saved {
	font-family: RobotoBold;
	font-size: 27px;
	color: var(--brandMedGreen);
	border-top: solid 1px #f7f7f7;
	text-align: right;
	padding-right: 20px;
}

#baseline {
	border-left: solid 5px var(--brandRed);
	padding-left:10px;
}
#target {
	border-left: solid 5px var(--brandYellow);
	padding-left:10px;
}
#amb10 {
	border-left: solid 5px var(--brandLightBlue);
	padding-left:10px;
}
#amb25 {
	border-left: solid 5px var(--brandMedBlue);
	padding-left:10px;
} 
#amb50 {
	border-left: solid 5px var(--brandDarkBlue);
	padding-left:10px;
} 



</style>