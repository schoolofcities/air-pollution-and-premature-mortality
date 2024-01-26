<script>

import Select from "svelte-select";
import cmaSummary from "../data/cma-summary.json";

const sexAll = ["All", "Female", "Male"];
const pollutionAll = ["PM2.5 and NO2", "PM2.5", "NO2"];
const ageAll = ["All Ages", "18-24", "25-34", "35-44", "45-54", "55-64", "65-74"]

//alphabetize CMAs so the desired CMA is easy to find in the dropdown
let cmaAll = cmaSummary
	.sort( function (a, b) { 
		return a.cmaname < b.cmaname ? -1 : 1;
	})
	.map((item) => item.cmaname);

let cmanameSelected = "Toronto";
let sexSelected = "All";
let pollutionSelected = "PM2.5 and NO2"
let ageSelected = "All Ages"


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


function updateStats() {

}

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

	meow
	
</div>


<style>


p {
	font-family: RobotoBold;
	font-size: 14px;
	margin: 0px;
	margin-top: 5px;
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
}


</style>