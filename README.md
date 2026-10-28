# Over the Top: a Korean container ship through the Arctic

An animated globe of the **PanStar Acro**'s round trip from Busan to Europe and back through the Northern Sea Route (NSR), 22 August to 11 October 2026, with the sea ice of each day underneath. The ship is shown against a "ghost" ship that takes the usual route through the Suez Canal.

- **Page A, data globe** (works on phones): https://likrentai.github.io/skarcticroute/
- **Page B, 3D globe** (desktop only): https://likrentai.github.io/skarcticroute/cesium.html
- **Case study**: https://likrentai.github.io/skarcticroute/case-study.html (also as a [PDF](https://likrentai.github.io/skarcticroute/case-study.pdf))

## The voyage

According to Arctic Portal (23 October 2026, citing Reuters and the Centre for High North Logistics), the PanStar Acro was the first Korean commercial round trip to Europe through the NSR:

| Item | Value |
|---|---|
| Route | Busan, Felixstowe, Rotterdam, Gdansk, Busan |
| Dates | 22 Aug 2026 to 11 Oct 2026 (50 days) |
| Distance | 16,385 nm round trip |
| Arctic passage | about 6 days each way at about 15 knots |
| Cargo out | 737 TEU (1,300 TEU hoped for; capacity 2,758 TEU) |
| Cargo back | 1,743 TEU of empty containers |

## Important: the route is reconstructed

**This is not the ship's real track.** No AIS position data was used. The route was drawn by hand from waypoints along the usual NSR corridor (through the Bering Strait and round Cape Zhelaniya), with one steady speed and 1.5 days in each European port, so that the departure and return dates match the reports. Checks against the reported figures (notebook `00route`):

| Leg | Reported | Reconstructed | Difference |
|---|---|---|---|
| Busan to Felixstowe | 7,577 nm | 7,446 nm | -1.7% |
| Gdansk to Busan | 7,660 nm | 7,628 nm | -0.4% |
| Round trip | 16,385 nm | 16,034 nm | -2.1% |
| Suez comparison (westbound) | 10,877 nm | 10,793 nm | -0.8% |

On this reconstruction the ship reaches Felixstowe on day 21.1 against day 30.6 via Suez: about 9.5 days saved on a route about 31% shorter. Distances from the ship to the ice edge are measured from our track, so they show how close the corridor ran to the ice, not how close the real ship came.

## What is in the repository

```
00route.ipynb          route waypoints, timing, checks against reported figures -> docs/data/route.json
01seaice.ipynb         daily sea ice: download, ice outlines, daily pictures and numbers
docs/index.html        page A: MapLibre GL globe with deck.gl
docs/cesium.html       page B: CesiumJS 3D globe
docs/data/route.json   route, ports, events, facts
docs/data/seaice.json  daily ice outlines and numbers (page A, about 5 MB)
docs/data/seaice_days.json  daily numbers only (page B, about 6 KB)
docs/data/ice/*.png    one transparent ice picture per day (page B)
docs/data/ship.glb     simple low-poly ship model made for this project (Python, trimesh)
```

The raw daily sea ice files (`data/raw/seaice`, 51 GeoTIFFs) are not in the repository; `01seaice` downloads them again.

## Sea ice

Daily sea ice concentration from the AMSR2 satellite radiometer, ASI algorithm, 6.25 km grid, University of Bremen. Ice is drawn where concentration is 15% or more (the usual definition of the ice edge). Over the voyage the Arctic ice extent on this data fell to a minimum of 4.38 million km2 on 8 September 2026.

## Run it yourself

The pages are static files. To view them on your own computer:

```
cd docs
python -m http.server 8000
```

then open http://localhost:8000 (page A) or http://localhost:8000/cesium.html (page B). Each page has a SETTINGS block at the top (lines marked `// <-`) for colours, speed and camera.

Page B uses Cesium ion imagery (Bing Maps aerial) and needs a Cesium ion access token in `ionToken`. The token in this repository only works on https://likrentai.github.io. For your own copy, make a free token at https://ion.cesium.com, or set `imagery: "bluemarble"` or `"naturalearth"`, which need no account.

To rebuild the data, make an environment with Python 3.12 and run the notebooks in order:

```
uv venv --python 3.12
uv pip install jupyter numpy pandas matplotlib rasterio shapely contourpy pillow requests
```

## Credits and sources

- **Voyage facts**: Arctic Portal, "South Korean ship completes Arctic trade voyage", 23 October 2026, citing Reuters and the Centre for High North Logistics (CHNL). https://arcticportal.org/news-events/news/4175-south-korean-ship-completes-arctic-trade-voyage
- **Sea ice**: AMSR2 ASI sea ice concentration, University of Bremen, https://seaice.uni-bremen.de. Spreen, G., L. Kaleschke and G. Heygster (2008), Sea ice remote sensing using AMSR-E 89 GHz channels, J. Geophys. Res. 113, C02S03, doi:10.1029/2005JC003384. Dataset: Melsheimer, C. and G. Spreen (2019), AMSR2 ASI sea ice concentration data, Arctic, version 5.4, PANGAEA, doi:10.1594/PANGAEA.898399 (licence CC BY 4.0).
- **Page A imagery**: NASA Global Imagery Browse Services (GIBS), Blue Marble Shaded Relief and Bathymetry. https://www.earthdata.nasa.gov/gibs
- **Page B imagery and globe**: CesiumJS and Cesium ion (https://cesium.com), with Bing Maps aerial imagery from Microsoft via Cesium ion.
- **Libraries**: MapLibre GL JS, deck.gl, CesiumJS; in Python: rasterio, shapely, contourpy, pandas, NumPy, Matplotlib, Pillow.

## Licence

- **Code** (the notebooks and the two web pages): MIT licence, see `LICENSE`.
- **Data made in this project** (`docs/data`: route, sea ice outlines, daily ice pictures and numbers, ship model): Creative Commons Attribution 4.0 (CC BY 4.0). The sea ice files are derived from the University of Bremen AMSR2 data, which is itself CC BY 4.0, so please credit both this project and the University of Bremen (citations above).
- **Not covered**: the imagery shown on the pages (NASA GIBS, Bing Maps via Cesium ion) is streamed from its providers and stays under their own terms. Voyage facts are quoted from the news source above.

## Author
Lik Ren Tai