# Earth texture sources

Downloaded and visually inspected 2026-09-17. Original pixels are retained; no generative image processing was used. All textures are 2:1 equirectangular maps, north at the top, with the antimeridian seam at the sides. Day and night land features have matching longitude orientation.

## Files

| Local file | Pixels | Bytes | Source and purpose |
| --- | --- | ---: | --- |
| `day.jpg` | 5400 × 2700 | 2,308,163 | NASA Blue Marble: Next Generation, August 2004 cloud-free base map with shaded topography and bathymetry |
| `night.jpg` | 4096 × 2048 | 267,061 | Three.js official Earth example, night terrain and amber city lights |
| `clouds.png` | 1024 × 512 | 226,113 | Three.js official legacy Earth cloud overlay; PNG has an alpha channel |
| `surface.jpg` | 4096 × 2048 | 1,508,389 | Three.js official Earth example packed map: red = bump, green = roughness, blue = cloud density |
| `normal.jpg` | 2048 × 1024 | 336,774 | Three.js official Earth tangent-space normal map |
| `specular.jpg` | 2048 × 1024 | 223,421 | Three.js official Earth specular map; ocean light, land dark |

Raster total: 4,869,921 bytes (about 4.64 MiB).

## NASA day map

- Catalog: <https://science.nasa.gov/earth/earth-observatory/blue-marble-next-generation/base-topography-bathymetry/>
- Original file: <https://assets.science.nasa.gov/content/dam/science/esd/eo/images/bmng/bmng-topography-bathymetry/august/world.topo.bathy.200408.3x5400x2700.jpg>
- Credit: NASA Earth Observatory, Blue Marble: Next Generation.
- Usage: NASA imagery generally is not subject to US copyright and can be used factually for informational visualizations with acknowledgment. NASA endorsement must not be implied. See <https://www.nasa.gov/nasa-brand-center/images-and-media/> for the complete guidelines.

## Three.js textures

Official repository directory: <https://github.com/mrdoob/three.js/tree/dev/examples/textures/planets>

Original files:

- `night.jpg`: <https://raw.githubusercontent.com/mrdoob/three.js/dev/examples/textures/planets/earth_night_4096.jpg>
- `clouds.png`: <https://raw.githubusercontent.com/mrdoob/three.js/dev/examples/textures/planets/earth_clouds_1024.png>
- `surface.jpg`: <https://raw.githubusercontent.com/mrdoob/three.js/dev/examples/textures/planets/earth_bump_roughness_clouds_4096.jpg>
- `normal.jpg`: <https://raw.githubusercontent.com/mrdoob/three.js/dev/examples/textures/planets/earth_normal_2048.jpg>
- `specular.jpg`: <https://raw.githubusercontent.com/mrdoob/three.js/dev/examples/textures/planets/earth_specular_2048.jpg>

The current [Three.js Earth example](https://github.com/mrdoob/three.js/blob/dev/examples/webgpu_tsl_earth.html) explicitly credits **Solar System Scope (resized and merged)** for its 4096-pixel textures. Solar System Scope's original planetary textures are based on NASA data and are distributed under **Creative Commons Attribution 4.0 International**, permitting adaptation and commercial use with attribution. Source/license: <https://www.solarsystemscope.com/textures/> and <https://creativecommons.org/licenses/by/4.0/>. Therefore credit `night.jpg` and `surface.jpg` to **Solar System Scope / NASA, adapted in the Three.js examples, CC BY 4.0**. The local copies have not been modified.

The older `clouds.png`, `normal.jpg`, and `specular.jpg` are unmodified files in the official Three.js examples repository. No individual creator/license file accompanies them in that directory; do not attribute these older files to Solar System Scope without further provenance. The repository's MIT license is retained locally as `THREE-LICENSE.txt`; <https://github.com/mrdoob/three.js/blob/dev/LICENSE>.

## Rendering notes

- `day.jpg` and `night.jpg` are color textures: use sRGB color space.
- Normal, specular, packed surface, and alpha/density values are data: do not apply sRGB decoding.
- `clouds.png` is a white RGBA overlay. Use as a transparent material's color map so its alpha is respected; avoid blindly using its white RGB as an alpha map.
- For a higher-detail moving cloud layer, prefer the **blue channel of `surface.jpg`** in a custom shader. The upstream example uses `smoothstep(0.2, 1.0, blue)` for cloud strength. Its red and green channels are other surface data, so the packed file must not be used directly as a grayscale alpha map.
- For gentle cloud motion, offset only the cloud UV longitude and wrap horizontally; keep terrain and night UVs aligned.
- These are historic visualization textures, not live weather, current lighting measurements, or a geographic data layer for analysis.

## Download verification

The five Three.js files were verified against the Git blob SHA values returned by the official GitHub contents API:

| File | Git blob SHA-1 |
| --- | --- |
| `night.jpg` | `d5cfb85a9aba6f42d637d88cef4171f610abc0fe` |
| `clouds.png` | `3f17c2123b7e2afb43735c901de128543e8bab08` |
| `surface.jpg` | `155558847a495729e7d5200fb07c0e93ffdddbd1` |
| `normal.jpg` | `5bf40df630d9ae7cb8ae026c930b8f3421bbec54` |
| `specular.jpg` | `06e03a3bfc7fe64f711175fe2c66bfb76b61cd0d` |
