# Maps and Geocoding

The common maps vocabulary -- the geocodes that turn addresses into coordinates and back, the places and points of interest on the map, the routes between them and the distance matrices that compare many at once -- distilled from the APIs that already exist. Maps APIs are operations more than resources, so each concept is the shape of a result. Part of [Schema Commons](https://github.com/api-commons/schema-commons),
published by the [API Commons](https://apicommons.org).

Built from the providers tagged Geocoding, Mapping, Maps, GIS, Routing, Location, Location Intelligence, Navigation, Places, Geospatial in [APIs.io](https://apis.io) that
publish their own OpenAPI. Only harvested contracts count -- nothing API Evangelist
generated. Every number below is a count of providers.

## What the market already agrees on

<!-- commons:summary -->
| Concept | Expose it | Readable schema | Core properties (share) | Collection named |
| --- | ---: | ---: | --- | --- |
| geocode | 22 | 21 | address 81%, name 67%, country 62%, confidence 57%, geometry 57%, id 52%, latitude 52%, longitude 52% | reverse 14, geocode 8, search 3, forward 3 |
| place | 25 | 24 | name 75%, id 67%, geometry 58%, address 54% | places 10, place 5, poi 4, pois 4 |
| route | 15 | 13 |  | route 7, routes 6, directions 3, routing 1 |
| distance_matrix | 9 | 8 | destinations 88%, origins 88%, id 50%, units 50% | matrix 5, sources_to_targets 3, distance-matrix 1, matrices 1 |

Cohort: 193 providers with harvested OpenAPI, 45 crosswalks. Of the providers exposing a geocode: 23% create, 86% list, 4% fetch, 0% update, 0% delete. Generated 2026-11-10 by build.py.
<!-- /commons:summary -->

Shares are of the providers with a readable resource schema for that concept. The
extended tier and the long tail of every concept are in [profile.yml](profile.yml).

## What the measurement shows

- A geocode agrees on an address, a name, a country, a confidence score, coordinates and a geometry -- once GeoJSON is read as GeoJSON. Pelias-style geocoders return FeatureCollections whose fields live under each Feature's `properties`.
- A place agrees on id, name, geometry and address; categories and opening hours are modelled by too few to share.
- A route agrees on nothing: routing engines publish their request options (costing, profiles, avoidances) far more than a result shape, and Valhalla, OSRM and the commercial APIs each return legs and geometry differently.
- A distance matrix agrees on its origins and destinations.

## Artifacts

- [openapi.yml](openapi.yml) -- OpenAPI 3.1 of the shared operations. No one serves it; it
  is the shape to map to. Lints clean against the
  [naming ruleset](https://github.com/api-commons/schema-commons/blob/main/ruleset/schema-commons-naming.yaml).
- [schema/](schema) -- JSON Schema 2020-12 per concept; each property carries
  `x-commons-tier`, `x-commons-share`, `x-commons-providers` and the spellings seen.
- [crosswalk/](crosswalk) -- one per provider, common name -> their name, with the harvested
  spec and operations each mapping was read from.
- [adopters.yml](adopters.yml) -- who exposes which concept, linked to their APIs.io page.
- [concepts.yml](concepts.yml) -- the only hand-written input: concepts, path segments and
  their qualifiers, aliases, reviewed exclusions, merged duplicates, and the decisions below.
- [census/](census) -- the cohort and the raw census the profile is built from.

## Decisions

These are calls, not measurements, recorded so they can be revisited as the method matures.

- Maps concepts are results, not resources: a geocode is what `/geocode`, `/reverse` and address search return.
- GeoJSON is read as a standard: the census unwraps `features` and lifts a Feature's `properties` beside its geometry (a census change that applies to every context).
- Points of interest, nearby search and place autocomplete are places; directions and route optimisation are routes; sources-to-targets is a distance matrix.
- A reverse DNS lookup and Vastu compass directions are not maps (excluded by name).
- Duplicates merged: Bee Maps into Hivemapper.

## Rebuild

```bash
../schema-commons/scripts/run.sh .
```

## License

Artifacts CC BY-NC-SA 4.0 ([LICENSE](LICENSE)); code Apache 2.0 ([LICENSE-CODE](LICENSE-CODE)).
