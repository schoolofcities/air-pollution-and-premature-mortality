<script>

    import TopSofC from "../lib/TopSofC.svelte";
    import Map from "../lib/Map.svelte";
    import Results from "../lib/Results.svelte";
    import "../assets/global-styles.css";
    import topImage from "../assets/top-image.png"
    
</script>

<TopSofC/>

<main>

    <div class="top-image">
        <img src="{topImage}" alt="Your Image Description">
    </div>
    
    <div class="text">

        <div class="title">
            <h1>Air Pollution and Premature Mortality </h1>
            <p>Author Names, Date, Funding Notes, etc.</p>
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
        
    </div>

    <div class="text">


        <h3>Acknowledgements</h3>

        <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
        </p>
        <p>
            Photograph of Calgary by Jeff Allen.
        </p>

       

        <h3>Data sources and methods</h3>
       
       
        <p> PM2.5 data are from <a href = 'https://sites.wustl.edu/acag/datasets/surface-pm2-5/#V5.GL.04' target = 'blank'>van Donkelaar et al. (2019)</a> V4.NA.03. We averaged the mean annual PM2.5 values from 2016 and 2017.
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
			This webpage uses the JavaScript framework <a href='https://svelte.dev/' target = 'blank'>Svelte</a>. Maps were created using <a href='https://d3js.org/' target = 'blank'>D3</a> and <a href='https://maplibre.org/' target = 'blank'>MapLibre GL JS</a>.
		</p>
        <p>
			Data and code for this work are on <a href='https://github.com/schoolofcities/air-pollution-and-premature-mortality' target = 'blank'>GitHub</a>.
		</p>

    



</main>

<style>

    
</style>

