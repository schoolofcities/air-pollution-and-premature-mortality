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
			<b><u>Writing</u></b>: Konstantine Diochnos
			<br>
			<b><u>Maps & Graphics</u></b>: Jeff Allen, Emily Sakaguchi</p>
			<p>Funded by a <a href="https://schoolofcities.utoronto.ca/programs-opportunities/urban-challenge-grant/" target = 'blank'>School of Cities Urban Challenge Grant</a>. Read the full paper here.</p>
		</div>

		<p>
			Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
		</p>

		<p>
			At vero eos et accusamus et iusto odio dignissimos ducimus, qui blanditiis praesentium voluptatum deleniti atque corrupti, quos dolores et quas molestias excepturi sint, obcaecati cupiditate non provident, similique sunt in culpa, qui officia deserunt mollitia animi, id est laborum et dolorum fuga. Et harum quidem reru[d]um facilis est e[r]t expedita distinctio. Nam libero tempore, cum soluta nobis est eligendi optio, cumque nihil impedit, quo minus id, quod maxime placeat facere possimus, omnis voluptas assumenda est, omnis dolor repellend[a]us. Temporibus autem quibusdam et aut officiis debitis aut rerum necessitatibus saepe eveniet, ut et voluptates repudiandae sint et molestiae non recusandae. Itaque earum rerum hic tenetur a sapiente delectus, ut aut reiciendis voluptatibus maiores alias consequatur aut perferendis doloribus asperiores repellat.
		</p>

		<h3>Air Pollution in Canadian Urban Regions</h3>
		
		<p>
			Temporibus autem quibusdam et aut officiis debitis aut rerum necessitatibus saepe eveniet, ut et voluptates repudiandae sint et molestiae non recusandae. 
		</p>
		
	</div>

	<Map/>    

	<div class="text">

		<p>
			Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
		</p>

		<h3>Air Pollution and Premature Mortality</h3>

		<p>
			At vero eos et accusamus et iusto odio dignissimos ducimus, qui blanditiis praesentium voluptatum deleniti atque corrupti, quos dolores et quas molestias excepturi sint, obcaecati cupiditate non provident, similique sunt in culpa, qui officia deserunt mollitia animi, id est laborum et dolorum fuga. Et harum quidem reru[d]um facilis est e[r]t expedita distinctio. Nam libero tempore, cum soluta nobis est eligendi optio, cumque nihil impedit, quo minus id, quod maxime placeat facere possimus, omnis voluptas assumenda est, omnis dolor repellend[a]us. 
		</p>

		<p>
			Temporibus autem quibusdam et aut officiis debitis aut rerum necessitatibus saepe eveniet, ut et voluptates repudiandae sint et molestiae non recusandae. Itaque earum rerum hic tenetur a sapiente delectus, ut aut reiciendis voluptatibus maiores alias consequatur aut perferendis doloribus asperiores repellat.
		</p>
		
		<Results/>

		<p>
			Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
		</p>

		<Totals/>

		<p>
			At vero eos et accusamus et iusto odio dignissimos ducimus, qui blanditiis praesentium voluptatum deleniti atque corrupti, quos dolores et quas molestias excepturi sint, obcaecati cupiditate non provident, similique sunt in culpa, qui officia deserunt mollitia animi, id est laborum et dolorum fuga. Et harum quidem reru[d]um facilis est e[r]t expedita distinctio. Nam libero tempore, cum soluta nobis est eligendi optio, cumque nihil impedit, quo minus id, quod maxime placeat facere possimus, omnis voluptas assumenda est, omnis dolor repellend[a]us. 
		</p>

		<p>
			Temporibus autem quibusdam et aut officiis debitis aut rerum necessitatibus saepe eveniet, ut et voluptates repudiandae sint et molestiae non recusandae. Itaque earum rerum hic tenetur a sapiente delectus, ut aut reiciendis voluptatibus maiores alias consequatur aut perferendis doloribus asperiores repellat.
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
   

		<h3>Data sources and methods</h3>
	   
		<p>Note any important details here about the premature mortality data that are not included above.</p>
	   
		<p>PM2.5 data are from <a href = 'https://sites.wustl.edu/acag/datasets/surface-pm2-5/#V5.GL.04' target = 'blank'>van Donkelaar et al. (2019)</a> V4.NA.03. We averaged the mean annual PM2.5 values from 2016 and 2017.
		</p>

		<p>
			Nitrogen dioxide data from 2016 were indexed to DMTI Spatial Inc. postal codes, which were provided by the Canadian Urban Environmental Health Research Consortium (<a href = 'https://www.canuedata.ca/metadata.php' target = 'blank'>CANUE</a>). The point data were interpolated using IDW where k = 2 using the same cell size as the PM2.5 data (0.01° × 0.01°).
			PM2.5 and NO2 data were converted from raster to vector data and added incorporated into a single geojson file via spatial join.       
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

