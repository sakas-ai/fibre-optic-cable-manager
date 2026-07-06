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
fibre count (2–288F), status (planned / active / fault), route colour, start/end
addresses, and free-text notes.

### Logical (circuit) layer

On top of the physical cables sits a **logical connection** layer. A logical connection
is a single **abstract curve** (an arc from start A to end D) representing a customer
circuit that may run over several physical cables in sequence (A → B → C → D). Build one
with **🔗 Connect** by clicking cables in order; it stores customer, service/bandwidth,
status, colour, notes and a **per-cable fibre/strand assignment** (which fibres the circuit
uses in each cable, e.g. `1-2`, validated against each cable's fibre count). The curve does
not trace the cables; if a cable it uses is moved or deleted, the endpoints re-flow.

### Splice / joint layer

A third layer shows **splice/joint points**, auto-derived wherever cable ends meet:
a node where ≥2 cables meet is a **splice closure/joint**; a lone cable end is a
**termination**. Click a marker to name it, set its type (splice closure / joint box /
hand-hole / street cabinet / termination) and add notes. Joints recompute automatically
as cables are drawn, reshaped or deleted.

### DCIM site floor plans

Each cable's **start point** defines a **site** (a data centre / exchange / cabinet). From
the cable editor (**Open site floor plan**) or the **Sites** tab you open a full-screen
floor-plan editor — a second OpenLayers map on a pixel (non-geographic) projection — to lay
out the site's inside:

- **Rooms** and **capacity zones** (cooling / power / security / network, with utilisation),
- **Racks** (U height / used U, colour-coded by fill), **assets** (server / switch / router /
  patch panel / PDU / storage) and internal **cabling**,
- select / move (drag) / reshape (handles) / delete, per-layer visibility toggles, and a live
  capacity summary (rooms, racks, total & used U, assets, cabling).

The plan is measured in **centimetres** with the origin at the **top-left corner (0,0)**,
y increasing downward. A live cursor readout and a scale bar show real distances, and the
properties panel gives each rack/room an editable **position (X,Y from 0,0) and size (W×D)**
in cm — so a rack's exact location can be read off or typed in. Floor plans are stored per
site in `localStorage`, keyed to the cable start point.

### Capacity & fault-impact analysis

The fibre assignments feed a lightweight capacity/impact view:

- **Utilization** — each cable in the list shows a used/free fibre bar; the cable editor
  lists exactly which circuits consume which fibres, flagging **fibre clashes**
  (two circuits on the same fibre) and numbers that **exceed the cable's fibre count**.
- **Fault impact** — mark a cable's status **Fault** and every customer circuit that runs
  over it is flagged **DOWN**, drawn in an alarm style on the map, with a banner showing
  `⚠ N cable faults · M circuits affected` (click it to jump to the affected connections).

Every sidebar tab (Cables / Logical / Joints / Sites) has a **search box**, a
**result count**, and **incremental rendering** (rows load in batches as you scroll),
so the lists stay fast with thousands of cables, connections and sites.

Extras: four-tab sidebar with per-layer visibility toggles,
auto-save to `localStorage`, GeoJSON import/export of cables + logical connections
(EPSG:4326), snapping, hover-to-edit vertices, Nominatim reverse-geocoding of endpoints,
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
