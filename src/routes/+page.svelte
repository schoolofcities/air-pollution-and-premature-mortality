<script>

	import TopSofC from "../lib/TopSofC.svelte";
	import Map from "../lib/Map.svelte";
	import Results from "../lib/Results.svelte";
	import Totals from "../lib/Totals.svelte";
	import "../assets/global-styles.css";
	import topImage from "../assets/top-image.png"
	
</script>



<svelte:head>
	
	<meta
		name="viewport"
		content="width=device-width, initial-scale=1, minimum-scale=1"
	/>

</svelte:head>



<TopSofC/>

<main>

	<div class="top-image">
		<img src="{topImage}" alt="Your Image Description">
	</div>
	
	<div class="text">

		<div class="title">
			<h1>Air Pollution and Premature Mortality </h1>
			<p><b><u>Research</u></b>: Sabrina Chiodo; Lief Pagalan; Mackenzie Hurst; Meghan O’Neill; Helen Stylianou; Lori M. Diemert; Hong Chen; Jeffrey R. Brook; Andy Hong; Laura C. Rosella<br>
			<b><u>Writing</u></b>: Kosta Diochnos
			<br>
			<b><u>Maps & Graphics</u></b>: Jeff Allen, Emily Sakaguchi</p>
			<p>Funded by a <a href="https://schoolofcities.utoronto.ca/programs-opportunities/urban-challenge-grant/" target = 'blank'>School of Cities Urban Challenge Grant</a>. Read the full paper here.</p>
		</div>

		<p>
			Climate change is the largest global health threat of the 21st century, and one of the most important measures that can be taken to lessen its toll on human health is to reduce air pollution. 
		</p>
		<p>
			Cities are home to most of the world’s population and account for <a href="https://www.nature.com/articles/525179a">more than 70%</a> of global greenhouse gas (GHG) emissions. With increasing urbanization, traffic, economic activity, and extreme weather effects like smoke from wildfires, cities become areas of elevated air pollution. In Canada, <a href="https://canue.ca/pm-2-5-what-it-is-and-why-it-matters-february-24th-2022/" target="_blank">86% of the population</a> lives in areas where outdoor air pollutants exceed standard limits set by the World Health Organization. <a href="https://www.foss4gna.org/" target="_blank">Health Canada</a> has estimated that 15,300 premature deaths (i.e. deaths that occur before age 75) occur annually as a direct result of air pollution from human activity. 
		</p>
		<p>
			To understand the impact of reducing air pollution on premature deaths, we used a tool called the <a href="https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7640636/" target="_blank">Mortality Population Risk Tool (PreMPoRT)</a> to simulate how reductions in nitrogen dioxide (NO2) and fine particulate matter less than 2.5 μm in diameter (PM2.5) would impact premature mortality in 31 Canadian Census Metropolitan Areas (CMAs). PM2.5 is a main component of harmful air pollution, while NO2 is another pollutant that can exacerbate the effects of air pollution on human health.
		</p>
		<p>
			Our results show that interventions aimed at reducing PM2.5 and NO2 could decrease the number of premature deaths for Canadian cities. The greater the air pollution intervention, the greater the reduction in predicted premature deaths.
		</p>

		<h3>Total lives saved by reducing air pollution</h3>

		<Totals/>

		<p>
			Specifically we simulated three different air pollution reduction scenarios: reducing PM2.5, reducing NO2, and reducing PM2.5 and NO2 simultaneously. We also modelled four variations in air pollution intervention scenarios:
		</p>
		<p>	
			1) Meeting the Ambient Air Quality Standard: In this baseline scenario, we capped observed air pollution exposures to a maximum of 8.8 μg/m3 for annual mean PM2.5 and 12.0 ppb for annual mean NO2. These are the maximum annual average exposures set by Canadian Ambient Air Quality Standards.
			
		</p>
		<p>	
			2) 10% reduction + meeting the Ambient Air Quality Standard: In this scenario we reduced pollutant exposure levels by 10% for the entire population, and then capped exposure for anyone still above the standard to meet it. 
		</p>
		<p>	
			3) 25% reduction + meeting the Ambient Air Quality Standard: In the third scenario we reduced pollutant exposure levels by 25% for the entire population, and then capped exposure for anyone still above the standard to meet it.
		</p>
		<p>	
			4) 50% reduction + meeting the Ambient Air Quality Standard: In the third scenario we reduced pollutant exposure levels by 50% for the entire population, and then capped exposure for anyone still above the standard to meet it.
		</p>

		<!-- <br>
		<br>
		<br>
		<br>
		<br> -->

		<p>
			Air pollution levels and risk of premature mortality can vary by city. However, the overall predicted rates of premature death reflects the weighted average of predicted risk across all cities, masking city-specific patterns. To account for these variations, we also predicted city-specific rates of premature mortality and used Census population data to estimate the number of lives saved for each city.
		</p>

		<Results/>

		<p>
			Through strategic interventions such as transitioning to renewable energy sources, active travel and electrified public transit, and imposing strict emission standards on industrial facilities, Canada could improve its climate change resiliency and make significant improvements to public health. 
		</p>


		<h3>Air pollution within Canadian urban regions</h3>
		
		<!-- <p>	
			From the Canadian Ambient Air Quality Standards, maximum annual average exposures to 8.8 μg/m3 of PM2.5 and 12.0 ppb of NO2 are targets that help continuously improve air quality. 
		</p> -->
		<p>
			Across all CMAs, the observed average annual exposure of PM2.5 was 6.7 μg/m3 for males and females, and the observed average annual exposure of NO2 was 8.3ppb for females and 8.4ppb for males. However, 206,000 females and 202,000 males live in areas which exceed the PM2.5 standard, while 437,000 females and 488,000 males live in areas which exceed the NO2 standard. 
		</p>
		<p>
			The maps below show variations in exposure to PM2.5 and NO2 in 31 Canadian CMAs. Select from the dropdown to zoom to and show maps for different regions. 
		</p>
		
		
	</div>

	<Map/>    

	<div class="text">
<!-- 
		<p>
			The greatest reductions in premature mortality (over a five-year period) can be seen when intervening on both PM2.5 and NO2 pollutants simultaneously. Furthermore, the more that air pollution was reduced (i.e., 10% or 25% plus capping), the more that premature deaths could be prevented.
		</p> -->
		<p>	
			Notably, air pollution affects population demographics unequally. Areas of higher pollution exposure tend to have greater instability (i.e. decreased home security and ownership), a higher proportion of newcomers, and a lower proportion of employed and educated residents.
		</p>
		<p>
			Pollution reductions would be particularly beneficial for individuals in neighbourhoods with greater socioeconomic deprivation, housing instability, and proportions of racial minorities and recent immigrants. Additionally, by simulating reductions of pollution even below the Canadian air quality standards, we can see how even greater health benefits could occur across the population. 
		</p>

		


		
	</div>

	<div class="text">

<!-- 
		<h3>Acknowledgements</h3>

		<p>
			Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
		</p>
		<p>
			Postal code data were provided by DMTI Spatial Inc, via the Canadian Urban Environmental Health Research Consortium (CANUE) under the current SMART Agreement in place with Canadian Universities.
		</p>
		<p>
			Photograph of Calgary by Jeff Allen.
		</p> -->
   

		<!-- <h3>Data sources and methods</h3>
	   
		<p>
			For the maps, PM2.5 data are from <a href = 'https://sites.wustl.edu/acag/datasets/surface-pm2-5/#V5.GL.04' target = 'blank'>van Donkelaar et al. (2019)</a> V4.NA.03. We averaged the mean annual PM2.5 values from 2016 and 2017. Nitrogen dioxide data from 2016 were indexed to DMTI Spatial Inc. postal codes, which were provided by the Canadian Urban Environmental Health Research Consortium (<a href = 'https://www.canuedata.ca/metadata.php' target = 'blank'>CANUE</a>). The point data were interpolated using IDW where k = 2 using the same cell size as the PM2.5 data (0.01° × 0.01°). PM2.5 and NO2 data were converted from raster to vector data and added incorporated into a single geojson file via spatial join.       
		</p>
	  
		<p>
			The census metropolitan areas use the 2006 digital boundaries from <a href = 'https://www12.statcan.gc.ca/census-recensement/2011/geo/bound-limit/bound-limit-2006-eng.cfm' target = 'blank'>Statistics Canada</a>.
			The base map and reference layers are from <a href = 'https://protomaps.com/' target = 'blank'>Protomaps</a> and <a href = 'https://www.openstreetmap.org/#map=3/71.34/-96.82' target = 'blank'>OpenStreetMap</a>.
		</p>
		<p>
			This webpage uses the JavaScript framework <a href='https://svelte.dev/' target = 'blank'>Svelte</a>. Maps and charts were created using <a href='https://d3js.org/' target = 'blank'>D3</a> and <a href='https://maplibre.org/' target = 'blank'>MapLibre GL JS</a>.
		</p>
		<p>
			Data and code for this work are on <a href='https://github.com/schoolofcities/air-pollution-and-premature-mortality' target = 'blank'>GitHub</a>.
		</p>
		<p>
			The top photograph of Calgary was taken by Jeff Allen in 06/2023.
		</p> -->

		<!-- <h3>References</h3>
	   
		<p>
			CanMap Postal Code Suite v2015.3. [computer file] Markham: DMTI Spatial Inc., 2015.
		</p>
		<p>
			CanMap Postal Code Suite v2016.3. [computer file] Markham: DMTI Spatial Inc., 2016.
		</p>
		<p>
			Hammer, M. S.; van Donkelaar, A.; Li, C.; Lyapustin, A.; Sayer, A. M.; Hsu, N. C.; Levy, R. C.; Garay, M. J.; Kalashnikova, O. V.; Kahn, R. A.; Brauer, M.; Apte, J. S.; Henze, D. K.; Zhang, L.; Zhang, Q.; Ford, B.; Pierce, J. R.; and Martin, R. V., Global Estimates and Long-Term Trends of Fine Particulate Matter Concentrations (1998-2018)., Environ. Sci. Technol, doi: 10.1021/acs.est.0c01764, 2020
		</p>
		<p>
			Hystad P, Setton E, Cervantes A, Poplawski K, Deschenes S, Brauer M, et al. 2011. Creating National Air Pollution Models for Population Exposure Assessment in Canada. Environ. Health Perspect. 119:1123–1129; doi:10.1289/ehp.1002976.
		</p>
		<p>
			van Donkelaar, A., R. V. Martin, et al. (2019). Regional Estimates of Chemical Composition of Fine Particulate Matter using a Combined Geoscience-Statistical Method with Information from Satellites, Models, and Monitors. Environmental Science & Technology, 2019, doi:10.1021/acs.est.8b06392.
		</p>
		<p>
			Weichenthal S, Pinault L, Burnett RT. (2017) Impact of Oxidant Gases on the Relationship Between Outdoor Fine Particulate Air Pollution and Nonaccidental, Cardiovascular, and Respiratory Mortality. Scientific Reports 7, Article number: 16401. Doi:10.1038/s41598-017-16770-y
		</p> -->
	 

	



</main>

<style>

	
</style>

