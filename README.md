# GeoMARSH: Chatham viewer

A lightweight web viewer for the GeoMARSH marsh prioritization grid, built with
[MapLibre GL JS](https://maplibre.org/) and [PMTiles](https://protomaps.com/docs/pmtiles),
hosted for free on GitHub Pages.

## Repo layout

```
geomarsh-viewer/
├── index.html
├── README.md
└── data/
    ├── classes.pmtiles     # solid class polygons + status-flag outlines (what you see)
    └── detail.pmtiles      # individual cells with all attributes (click popups)
```

## Features

- On/off switches for Current prioritization, Future prioritization, and Suitability
- Swipe compare between any two layers
- Highlight chips for the status flags (Restricted, Ditch, Phragmites, Unvegetated, Marsh,
  Accommodation): turn one or more on to draw those areas on top of the map at any zoom
- Click a cell (zoomed in) for all component scores and status flags; the cell is outlined
- Opacity slider

## Building / updating the data

1. In R, run `download_geomarsh_layers.R`, then `make_display_layers.R`.
2. In Terminal, `cd` into the `geomarsh_data` folder and run:

   ```bash
   # Solid class polygons, all zoom levels
   tippecanoe -o classes.pmtiles -Z11 -z16 --force \
     --no-tile-size-limit --no-feature-limit --detect-shared-borders \
     -L current:tiles_input/current_classes.geojson \
     -L future:tiles_input/future_classes.geojson \
     -L suitability:tiles_input/suitability_classes.geojson \
     -L flag_restricted:tiles_input/flag_restricted.geojson \
     -L flag_ditch:tiles_input/flag_ditch.geojson \
     -L flag_phrag:tiles_input/flag_phrag.geojson \
     -L flag_unvegetated:tiles_input/flag_unvegetated.geojson \
     -L flag_marsh:tiles_input/flag_marsh.geojson \
     -L flag_accommodation:tiles_input/flag_accommodation.geojson

   # Individual cells for click popups. -Z14 is the zoom where clicking a cell starts
   # to work; use a lower number to click from further out (bigger file).
   tippecanoe -o detail.pmtiles -l detail -Z14 -z16 --force \
     --no-tile-size-limit --no-feature-limit \
     tiles_input/current.geojson
   ```

3. Copy both `.pmtiles` files into this repo's `data/` folder (delete the old
   `geomarsh.pmtiles`) and upload to GitHub.

## Changing colors

Edit `CURRENT_COLORS`, `FUTURE_COLORS` and `SUITABILITY_COLORS` near the top of `index.html`.
Each list has five colors, for `< 0.5`, `0.5–0.6`, `0.6–0.75`, `0.75–0.9`, `> 0.9`.
Highlight colors are in the `FLAGS` list just below.

## Testing locally

From this folder in Terminal:

```bash
python3 -m http.server 8000
```

Then go to <http://localhost:8000>. Press Control-C to stop.
