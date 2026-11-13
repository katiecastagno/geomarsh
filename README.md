# GeoMARSH: Chatham viewer

A lightweight web viewer for the GeoMARSH marsh prioritization grid, built with
[MapLibre GL JS](https://maplibre.org/) and [PMTiles](https://protomaps.com/docs/pmtiles),
hosted for free on GitHub Pages.

## Repo layout

```
geomarsh-viewer/
├── index.html              # the viewer
├── README.md
└── data/
    └── geomarsh.pmtiles    # grid tiles (built with tippecanoe)
```

## Features

- Imagery basemap with the prioritization grid on top
- Switch between Current prioritization, Future prioritization, and Suitability
- Swipe to compare Current (left) vs Future (right)
- Click any cell for all component scores and status flags
- Grid opacity slider

## Updating the data

1. Re-run `download_geomarsh_layers.R` in R.
2. Rebuild the tiles in Terminal:

   ```bash
   tippecanoe -o geomarsh.pmtiles -l grid -Z11 -z17 --force \
     --drop-densest-as-needed --extend-zooms-if-still-dropping \
     tiles_input/current.geojson
   ```

3. Replace `data/geomarsh.pmtiles` in this repo and push.

## Testing locally

Opening `index.html` by double-clicking won't load the tiles (browsers block that).
Instead, in Terminal, from this folder:

```bash
python3 -m http.server 8000
```

Then go to <http://localhost:8000>. Press Control-C in Terminal to stop.
