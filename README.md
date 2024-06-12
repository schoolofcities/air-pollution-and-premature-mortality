# air-pollution-and-premature-mortality

For the maps, PM2.5 data are from [van Donkelaar et al. (2019)](https://sites.wustl.edu/acag/datasets/surface-pm2-5/#V5.GL.04) V4.NA.03. We averaged the mean annual PM2.5 values from 2016 and 2017. Nitrogen dioxide data from 2016 were indexed to DMTI Spatial Inc. postal codes, which were provided by the Canadian Urban Environmental Health Research Consortium ([CANUE](https://www.canuedata.ca/metadata.php)). The point data were interpolated using IDW where k = 2 using the same cell size as the PM2.5 data (0.01° × 0.01°). PM2.5 and NO2 data were converted from raster to vector data and added incorporated into a single geojson file via spatial join.

The census metropolitan areas use the 2006 digital boundaries from [Statistics Canada](https://www12.statcan.gc.ca/census-recensement/2011/geo/bound-limit/bound-limit-2006-eng.cfm). The base map and reference layers are from [Protomaps](https://protomaps.com/) and [OpenStreetMap](https://www.openstreetmap.org).

This webpage was built using Svelte. Maps and charts were created using D3 and MapLibre GL JS.


Developing locally
```
git clone https://github.com/schoolofcities/air-pollution-and-premature-mortality
cd air-pollution-and-premature-mortality
npm install
```

Local development:

```
npm run dev
```

