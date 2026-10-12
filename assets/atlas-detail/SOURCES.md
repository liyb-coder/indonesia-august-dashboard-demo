# Indonesia atlas geographic detail — V17

Downloaded and checked 2026-09-23. These are geographic reference layers, not project locations, a current road inventory, or live satellite imagery. No generative image processing or hand-invented geographic paths are used. The main homepage globe and its assets are unchanged.

## Final composition — V17.1 arc restored

The user's latest preference restores the curved Earth and restrained pale-blue atmospheric arc in the project atlas. Indonesia remains the detailed foreground; surrounding land is muted. The homepage globe is unchanged. Terrain, vector lines and city/project anchors share a spherical east/up/south basis tangent at 118°E, 2.5°S. A bounded oblique camera fits the full Indonesian archipelago, with zoom up to 2.6×. Raster shading is illustrative geographic relief, not surveyed elevation or project BIM.

The renderer loads `indonesia-relief.jpg`, `geography-mask.png`, `earth-context.jpg` and `geography.json`. Two transparent rim passes share the terrain sphere geometry; no bloom, render targets, automatic lighting cycle or additional animation loop is introduced. GPU textures remain about 81 MiB including mipmaps; the opaque retained canvas and bounded drawing buffer remain.

## Raster sources

| File | Size | Source / use |
| --- | --- | --- |
| `indonesia-relief.jpg` | 4096 × 2979; 874,820 bytes | NASA GIBS Blue Marble, August 2004, shaded relief + bathymetry. Unmodified regional WMS JPEG. |
| `earth-context.jpg` | 1024 × 512; 109,836 bytes | Lanczos reduction of the existing NASA `../earth/day.jpg`; quiet global context outside the regional raster. |
| `geography-mask.png` | 2048 × 1489; 302,455 bytes | Rasterized Natural Earth 1:10m country polygons. R = Indonesia, G = all land, B = 1.6px blurred land mask for a restrained graphic coastline halo. Data texture, not sRGB. |

NASA [layer metadata](https://gibs.earthdata.nasa.gov/layer-metadata/v1.0/BlueMarble_ShadedRelief_Bathymetry.json) identifies the underlying imagery as **August 2004**, MODIS / NASA Earth Observatory. It is not an observation from the reporting month. NASA [GIBS access documentation](https://nasa-gibs.github.io/gibs-api-docs/access-basics/) and [imagery usage guidelines](https://www.nasa.gov/nasa-brand-center/images-and-media/). Credit: **NASA Earth Observatory / MODIS, Blue Marble**. No endorsement is implied.

Exact raster request:

`https://gibs.earthdata.nasa.gov/wms/epsg4326/best/wms.cgi?SERVICE=WMS&VERSION=1.1.1&REQUEST=GetMap&LAYERS=BlueMarble_ShadedRelief_Bathymetry&STYLES=&SRS=EPSG:4326&BBOX=87,-25,153,23&WIDTH=4096&HEIGHT=2979&FORMAT=image/jpeg`

### Spatial registration

- CRS: WGS84 / EPSG:4326; longitude first.
- Extent: **87°E–153°E, 25°S–23°N**. Raster north is at the top.
- Raster coverage is a 66° × 48° rectangular WMS request. The JPEG has about 62 pixels per degree, approximately 1.8km per output pixel near the equator. Output spacing is not a claim of native sensor resolution.
- The previous global 5400px atlas day map supplied 15 pixels per degree: regional sampling now has **4.1× the linear pixel density**.
- Shader UV: `u=(lon-87)/66`, `v=(lat+25)/48`; Three.js texture upload flips the north-up source so `v=1` is north. Country mask uses exactly the same extent.
- All visible layers use the shared spherical basis above. No stored point coordinates, island anchors, or city anchors changed.
- Elevation shading is present in NASA's geographic raster. The mesh supplies only Earth curvature; no fabricated mountain heights or project terrain coordinates were introduced.

## Vector sources

Natural Earth supplies [public domain](https://www.naturalearthdata.com/about/terms-of-use/) small-scale cartographic data. The official dataset pages link to its [maintainer repository](https://github.com/nvkelso/natural-earth-vector). The following master-branch source snapshots were downloaded on 2026-09-23; SHA-256 fingerprints below identify the exact inputs.

| Input | Official documentation | Download |
| --- | --- | --- |
| Country polygons | [Admin 0 countries](https://www.naturalearthdata.com/downloads/10m-cultural-vectors/10m-admin-0-countries/) | `https://raw.githubusercontent.com/nvkelso/natural-earth-vector/master/geojson/ne_10m_admin_0_countries.geojson` |
| Province boundary lines | [Admin 1 states/provinces](https://www.naturalearthdata.com/downloads/10m-cultural-vectors/10m-admin-1-states-provinces/) | `https://raw.githubusercontent.com/nvkelso/natural-earth-vector/master/geojson/ne_10m_admin_1_states_provinces_lines.geojson` |
| Rivers/lake centerlines | [Rivers](https://www.naturalearthdata.com/downloads/10m-physical-vectors/10m-rivers-lake-centerlines/) | `https://raw.githubusercontent.com/nvkelso/natural-earth-vector/master/geojson/ne_10m_rivers_lake_centerlines.geojson` |
| Roads | [Roads](https://www.naturalearthdata.com/downloads/10m-cultural-vectors/roads/) | `https://raw.githubusercontent.com/nvkelso/natural-earth-vector/master/geojson/ne_10m_roads.geojson` |

`geography.json` retains these reference themes:

- Indonesia coastline / national outline: 264 parts, **12,742 vertices**; neighboring outlines: 462 parts, 27,902 vertices.
- Internal boundaries: 66 source features with `ADM0_A3=IDN`, 226 component lines, 1,503 vertices.
- Rivers: 14 source features, 17 retained parts, 1,406 vertices. Source names include Kapuas, Mahakam, Kayan, Barito, Mamberamo and Digul.
- Roads: 413 source features, 414 retained parts, 5,733 vertices. Selection is `featurecla=Road`, rank ≤7, excluding ferry routes. Many international source features have no road name or classification; they are shown only as subdued geographic linework, without claiming highway status.

Processing: spatially select regional country polygons, rasterize land/Indonesia masks, clip linework to the documented region, retain domestic river/road segments by midpoint in the country mask (one-pixel dilation accommodates generalization mismatch), Douglas–Peucker simplify at 0.003°, round coordinates to 5 decimal places, then subdivide source segments to at most 0.25° so 3D chords follow the sphere. The precision is a storage choice, not survey accuracy. No line is created to connect a project or city.

Limitations: Natural Earth is a generalized 1:10,000,000 cartographic reference. Province boundaries may predate recent administrative changes; roads do not establish current existence, construction status or completeness. River/road clipping near international boundaries has the raster mask's approximately 3.6km pixel tolerance. These layers should not be used for navigation, legal boundaries or project siting.

## Reproduction and fingerprints

Download the inputs as `terrain.jpg`, `countries.geojson`, `admin1.geojson`, `rivers.geojson`, and `roads.geojson`, then run:

`python3 scripts/atlas-detail/build_assets.py /path/to/downloaded-sources`

The builder requires Pillow and uses the existing `public/assets/earth/day.jpg` for the reduced context. Inputs are not shipped because only the clipped local assets are needed at runtime.

| Input | SHA-256 |
| --- | --- |
| countries.geojson | `239eec57ac17f100a11e2536cffc56752c318b50ae765b0918ff7aab4ce8f255` |
| admin1.geojson | `1a1f30ccaaf4cc9c4bde34266f0b8cbb955d3a4cf254b756912255f2ec7c75b6` |
| rivers.geojson | `bb854a900ecbd3b408df46d5e16e3e0f974ba55993f9d8b5c26e855273c0905a` |
| roads.geojson | `66a0c7b438e92fd124822cc5921cfa11042f48c294ade5e0f03f2c6640fd0248` |
| terrain.jpg / indonesia-relief.jpg | `15c964d5f70381232a05c19d7a1f43d06e905ab3cb87737446fbc2616796c254` |

Runtime assets total **2,303,480 bytes (2.20MiB)**. The three RGB textures use at most approximately **81MiB** of RGBA8 GPU texture memory including mipmaps; vector positions add under 1.2MiB. One sphere geometry is shared by terrain and atmosphere. Geographic layers use four line draws; no HDR targets or bloom are added. WebGL stays on demand, direct, opaque and retained. Quiet foreign land keeps Indonesia the focus and reduces Australian foreground brightness.

Validation: geographic bounds / finite coordinates, all major island coverage, subdivision length, source feature presence and asset budget are checked in `tests/atlas-geography.test.mjs`. Packed mask spot checks cover Java, Sumatra, Kalimantan, Papua, Java Sea and Australia. Independent browser preview confirmed rendered mountain/bathymetry texture, inland linework, zoom and no WebGL console errors. Existing point-layout / RAF lifecycle / scene-transition tests remain unchanged.
