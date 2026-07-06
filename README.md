# Fibre Optic Cable Manager

A single-page web app for drawing and managing telecom fibre-optic cable routes on an
[OpenStreetMap](https://www.openstreetmap.org/) base map, built with the
[OpenLayers](https://openlayers.org/) toolkit.

![OpenLayers](https://img.shields.io/badge/OpenLayers-10-1f6feb) ![No build step](https://img.shields.io/badge/build-none-brightgreen)

## Features

Full **CRUD** over cable routes, entirely in the browser:

- **Create** – draw a route point-by-point (double-click to finish); a properties dialog opens automatically.
- **Read** – sidebar list with colour, status, type, fibre count and computed length; click to zoom. Live total-length readout.
- **Update** – drag route vertices in *Modify* mode; double-click a cable to edit its properties.
- **Delete** – remove a selected cable from the toolbar or the editor.

Each cable stores: name/ID, cable type (OS2 / OM3 / OM4 / ADSS / armoured / micro-duct),
fibre count (2–288F), status (planned / active / fault), route colour and free-text notes.

Extras: auto-save to `localStorage`, GeoJSON import/export (EPSG:4326), snapping,
dashed styling for *planned* routes and status-coloured endpoints.

## Running locally

It is a single static file — just open `index.html` in a browser, or serve the folder:

```bash
npx serve .
```

OpenLayers and the OSM tiles load from a CDN, so an internet connection is required.

## Notes

- The demo uses the public OSM tile layer, which has a [usage policy](https://operations.osmfoundation.org/policies/tiles/).
  For production, use your own tile source or a provider key.
- Data is stored per-browser in `localStorage`. Use **Export** to back up as GeoJSON.
