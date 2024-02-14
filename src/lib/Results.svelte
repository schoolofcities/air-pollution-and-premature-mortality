<script>

import Select from "svelte-select";
import predictedDeaths from "../data/predicted-deaths-results.json";
import * as d3 from "d3";

// options for dropdowns / selections
const sexAll = ["All", "Female", "Male"];
const pollutionAll = ["PM2.5 and NO2", "PM2.5", "NO2"];
const ageAll = ["All Ages", "18-24", "25-34", "35-44", "45-54", "55-64", "65-74"]
let cmaAll = [...new Set(predictedDeaths.map(item => item.CMA))];

// initial variables
let cmanameSelected = "Toronto";
let sexSelected = "All";
let pollutionSelected = "PM2.5 and NO2"
let ageSelected = "All Ages"

// log what is selected
console.log(predictedDeaths);

// update variables when selected
function selectCmaValue(e) {
	cmanameSelected = e.detail.value;
}
function selectSexValue(e) {
	sexSelected = e.detail.value;
}
function selectAgeValue(e) {
	ageSelected = e.detail.value;
}
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
	if (sex === "All" && age === "All Ages" ) {
		data = d3.rollups(
			data, 
			v => d3.sum(v, d => d["Predicted Premature Death"]), 
			d => d.Intervention, 
		);
	}
	else if (sex === "All" && age !== "All Ages") {
		data = data.filter(d => d.Age === age);
		data = d3.rollups(
			data, 
			v => d3.sum(v, d => d["Predicted Premature Death"]), 
			d => d.Intervention
		);
	}
	else if (sex !== "All" && age === "All Ages") {
		data = data.filter(d => d.SEX === sex);
		data = d3.rollups(
			data, 
			v => d3.sum(v, d => d["Predicted Premature Death"]), 
			d => d.Intervention
		);
	}
	else {
		data = data.filter(d => d.SEX === sex);
		data = data.filter(d => d.Age === age);
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

$: updateStats(cmanameSelected, sexSelected, pollutionSelected, ageSelected);

</script>



<div class="select-inputs">

	<div class="select-wrapper">
		<p>Census Metropolitan Area (CMA):</p>	
		<Select
			id="select"
			items={cmaAll}
			value={cmanameSelected}
			clearable={false}
			showChevron={true}
			on:input={selectCmaValue}
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
		<p>Pollution Type</p>	
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
	</div>

	<div class="select-wrapper">
		<p>Sex</p>	
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

</div>

<div class="results">

	<table>
		<tr class = column-header style = "text-align: left">
		  <th>Scenario</th>
		  <th >Premature<br>deaths</th>
		  <th>Estimated<br>lives saved</th>
		</tr>
		<tr class = deaths>
			<th></th>
			<th colspan = "2" class = "column-header" style = "font-size: 12px; font-style: italic">Per 100,000 people</th>
		</tr>
		<tr class>
			<td id = "baseline" class = "scenario-label" style="width: 220px">Baseline</td>
			<td class = "scenario-baseline">{data.Baseline.toFixed(0).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- premature deaths -->
		  </tr>
		<tr>
		  <td id = "target" class = "scenario-label" style="width: 220px">Reducing air pollutant levels to air quality standard targets</td>
		  <td class = "scenario-number">{data.Target.toFixed(0).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- premature deaths -->
		  <td class = "scenario-saved">{(data.Baseline - data.Target).toFixed(0).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- estimated lives saved -->
		</tr>
		<tr>
		  <td id = "amb10" class = "scenario-label" style="width: 220px">10% reduction + capping at the ambient air quality standards</td> 
		  <td class = "scenario-number">{data.AMB10.toFixed(0).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- premature deaths -->
		  <td class = "scenario-saved">{(data.Baseline - data.AMB10).toFixed(0).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- estimated lives saved -->
		</tr>
		<tr>
			<td id = "amb25" class = "scenario-label" style="width: 220px">25% reduction + capping at the ambient air quality standards</td>
			<td class = "scenario-number"> {data.AMB25.toFixed(0).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- premature deaths -->
			<td class = "scenario-saved">{(data.Baseline - data.AMB25).toFixed(0).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",")}</td> <!-- estimated lives saved -->
		  </tr>
	  </table>
	
</div>




<style>

p {
	font-family: RobotoBold;
	font-size: 14px;
	margin: 0px;
	margin-top: 5px;
}

table {
  border-collapse:separate;
  border-spacing: 15px 15px;
}

/* h3 {
	font-family: RobotoBold;
	font-size: 19px;
	font-style: italic;
	margin-bottom: 15px;
	color: var(--brandBlack);
	border-bottom: solid 1px var(--brandGray);
} */

/* .scenario {
	padding-left: 20px;
	margin-left: 5px;
	margin-right: 25px;
}
.scenario-text-wrapper {
	display: flex;
    justify-content: space-between;
	border-top: solid 1px #f7f7f7;
	margin-top: 2px;
	padding-bottom: 8px;
}
.scenario-text {
	margin-top: -5px;
	text-align: left;
}
.scenario-total {
	text-align: right;
}

.scenario-description {
	margin-top: -5px;
	font-family: RobotoRegular;
	font-size: 13px;
	color: var(--brandGray70);
}

*/
.column-header {
	font-family: RobotoRegular;
	font-size: 16px;
	margin-bottom: 15px;
	color: var(--brandBlack);
	vertical-align: bottom;
}
.scenario-label {
	font-family: RobotoRegular;
	font-size: 16px;
	border-top: solid 1px #f7f7f7;
}
.scenario-baseline {
	font-size: 25px;
	color: var(--brandRed);
	border-top: solid 1px #f7f7f7;
}
.scenario-number {
	font-size: 25px;
	color: var(--brandGray80);
	border-top: solid 1px #f7f7f7;
}
.scenario-saved {
	font-size: 25px;
	color: var(--brandMedGreen);
	border-top: solid 1px #f7f7f7;
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

.select-inputs {
	margin: 0 auto;
	width: 230px;
}
@media screen and (min-width: 500px) {
	.select-inputs {
		margin: 0 auto;
		width: 460px;
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
	}
  	.select-wrapper {
		width: 48%; /* Adjust the width as needed */
  	}
	.results {
		margin: 0 auto;
		width: 460px;
		display: flex;
		flex-wrap: wrap;
	}
}

</style>