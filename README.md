# ibf_viz, Antigua and Barbuda

Impact-Based Flood Forecast Viewer for Antigua and Barbuda: per forecast cycle warning maps
of the Tropical Storm Jerry hindcast (9 to 11 October 2025), produced by
`tito_utils.ibf_utils` chained on the seven parish flood map libraries of TITO.

Every building, road segment and parish is colour coded green, yellow, amber or red by the
Flood Risk Matrix (likelihood x potential impact severity) of the Scottish Flood Forecasting
Service and the UK Flood Guidance Statement, following Speight et al. (2018), Journal of
Flood Risk Management 11, S884-S901.

Live site: https://ahwalab.github.io/A-B_IBF/ (repository AHWALab/A-B_IBF). Companion
products: https://ahwalab.github.io/A-B_FIM/ and https://ahwalab.github.io/A-B_warnings/.

## Repository layout

- index.html : landing page
- antigua_barbuda/index.html : interactive warning map, 48 forecast cycles, self contained
- methods/index.html : method and design note for the island configuration
- .nojekyll : tells GitHub Pages to serve files as they are

The map page is fully self contained (Leaflet and all data are inlined, about 5 MB), so
the site has no build step and no dependencies. Basemap tiles load from public tile servers
when online; the warning layers work offline too.

## How the map is produced

The per cycle receptor products (`ibf_receptors.<cycle>.gpkg`, `ibf_admin_summary.<cycle>.csv`,
`ibf_summary.<cycle>.json` for each of the seven sites) come from the TITO orchestrator,
STEP 8. The viewer data are assembled by `ibf_10_data.py` of the training package pipeline
(`Country_trainings/A&B/web_interfaces/pipeline`: own parish rule for the overlapping site
windows, per cycle classification of the receptors that ever reach yellow, parish summaries,
FIM likelihood overlays), enriched with the OpenStreetMap extract of 6 April 2026 by
`ibf_08_osm.py` and `ibf_12_osm_merge.py` (named facilities and bridges or fords classified per
cycle from the likelihood mosaics, road and building names, land use context), and the page by
`ibf_40_build.py` from `ibf_template.html`.

## Basemap key

The CARTO raster basemaps (Light and Voyager) require an API key since 2026; without it the
tiles carry an "API KEY REQUIRED" watermark. The page carries the University of Iowa key in
the `CARTO_KEY` constant near the top of its script block. To change the key, edit that one
line in the built page, or in the template of the training package pipeline and rebuild.
The key is visible in the page source by design (client side tile requests); it can be
restricted to the site domain in the CARTO account settings. The OpenStreetMap and Esri
satellite basemaps do not use it.

## Enabling GitHub Pages (first time only)

1. Push this repository to GitHub (main branch).
2. On GitHub open Settings, then Pages.
3. Under Build and deployment choose Source: Deploy from a branch.
4. Select branch main and folder / (root), then Save.
5. Wait one or two minutes; the site appears at https://ahwalab.github.io/A-B_IBF/

## Version

Antigua and Barbuda edition, August 2026. See the method note for assumptions, caveats and
the open items for the next iteration.
