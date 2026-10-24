# Open World Data Census

> Status: Phase 1 bootstrap specification

## Goal

Build a continuously expanding, machine-readable census of **all lawfully public data resources we can discover**, regardless of publisher size, prestige, country, language, or domain.

This is a catalog first. It does **not** require mirroring every underlying dataset. The first job is to know that a resource exists, what it contains, where/when it covers, how it can be accessed, and how to return to the original data.

This work is additive to the upstream God's Eye View project. It must not imply authorship of upstream work.

## Inclusion rule

Index a resource when it is lawfully publicly discoverable or accessible through the public Internet, an open catalog, an open protocol, a public repository, a public cloud/object store, or a machine-readable public reference.

Access may be:

- `OPEN`
- `OPEN_AUTH_REQUIRED` (free registration/key)
- `TERMS_REQUIRED`
- `METADATA_ONLY`

Do not bypass authentication, access controls, paywalls, robots restrictions, or other technical/legal restrictions.

## No popularity cutoff

A national space agency and a small municipality's water-flow CSV are both in scope. So are universities, departments, labs, governments at every administrative level, public bodies, research institutes, NGOs, companies publishing open data, citizen-science networks, amateur sensor operators, open-source communities, and individuals publishing substantial public datasets.

## Phase 1 pipeline

`DISCOVER -> DESCRIBE -> DEDUPLICATE -> VERIFY -> EXPAND -> RECHECK`

Every discovered catalog, organization, repository, dataset, API, feed, sensor network, or reference can create new discovery candidates.

## Core resource states

- `DISCOVERED`
- `DESCRIBED`
- `VERIFIED`
- `INDEXED`
- `EXPANDED`
- `AUTH_REQUIRED`
- `TERMS_REQUIRED`
- `BROKEN`
- `UNCLASSIFIED`
- `RECHECK_DUE`

Discovery is intentionally never considered complete.

## Canonical resource record

```json
{
  "id": "stable internal id",
  "resource_type": "registry|catalog|repository|dataset|collection|api|feed|sensor_network|sensor|instrument|service|archive|object_store|organization|project|other",
  "title": "source title",
  "description": "source description",
  "publisher": {
    "name": "publisher name",
    "type": "government|municipality|university|institute|company|ngo|community|individual|unknown",
    "identifiers": [],
    "parent_id": null
  },
  "canonical_url": "",
  "endpoints": [],
  "protocols": [],
  "software": [],
  "formats": [],
  "languages": [],
  "topics": [],
  "spatial_coverage": [],
  "temporal_coverage": {"start": null, "end": null},
  "update_frequency": null,
  "live_capable": false,
  "historical_capable": false,
  "resolution": {},
  "license": {"id": null, "url": null, "status": "UNKNOWN"},
  "access": "OPEN|OPEN_AUTH_REQUIRED|TERMS_REQUIRED|METADATA_ONLY",
  "machine_readable": true,
  "parent_resources": [],
  "child_resources": [],
  "references": [],
  "discovered_via": [],
  "first_seen": "",
  "last_verified": "",
  "verification": {},
  "provenance": {},
  "fingerprints": {},
  "status": "DISCOVERED"
}
```

The schema must remain extensible. Domain-specific metadata must not be discarded merely because it does not fit the common fields.

## Bootstrap strategy

Do not begin by blindly crawling the entire Web. Import and union existing maps of the data ecosystem, then expand them recursively.

### Registry / meta-catalog roots

Initial root families include:

- Dateno Data Portals Registry / Common Data Index
- OpenAIRE Research Graph
- re3data
- OpenDOAR / ROAR
- FAIRsharing
- ROR and OpenAlex institution graphs
- DataCite / Crossref research-object graphs
- CKAN ecosystem/instance directories
- Socrata discovery
- ArcGIS Hub / public ArcGIS discovery
- STAC Index and STAC catalogs
- ODIS / OceanOPS / GOOS
- WMO WIGOS / OSCAR / WIS2
- FDSN data-center and station ecosystem
- GEOSS / Earth-observation catalogs
- NASA CMR / GCMD
- NOAA catalogs
- ESA / Copernicus catalogs
- IVOA astronomy registries
- GBIF / biodiversity federations
- national and supranational open-data catalogs
- open-source/community-curated public-data directories
- cloud open-data registries

This list is a seed set, never an allowlist.

## Protocol/platform discovery

A small number of adapters can expand many thousands of installations. Detect and enumerate at least:

- DCAT / DCAT-AP
- CKAN API
- Socrata Discovery/API
- ArcGIS REST / Hub
- STAC
- OGC API Records
- CSW
- WMS / WFS / WCS
- SensorThings
- SOS / SensorML / SOSA-SSN metadata
- GeoNetwork
- OAI-PMH
- Dataverse
- DSpace
- EPrints
- Invenio / Zenodo-family repositories
- ERDDAP
- THREDDS
- OPeNDAP
- SPARQL / RDF / Linked Data
- schema.org Dataset
- RSS / Atom
- REST / GraphQL discovery where documented
- public FTP / HTTP indexes / object-store catalogs where lawful

Unknown platform fingerprints go back into the discovery frontier rather than being dropped.

## Organization census

A second discovery path starts from organizations rather than known catalogs.

Build a global organization graph covering:

- countries and territories
- governments and ministries
- national agencies
- states/provinces/regions/counties
- municipalities and other local authorities
- public utilities and authorities
- all discoverable universities/higher-education institutions
- university faculties, institutes, labs, observatories and research centers
- public and private research institutes
- international organizations
- NGOs and foundations
- companies with public/open-data surfaces
- citizen-science and amateur communities
- individual public-data publishers

Compare this graph with known data resources. An organization with no discovered resources is a discovery candidate, not proof that it publishes no data.

## Long-tail discovery

Search beyond formal catalogs using:

- scholarly papers and their Data Availability / Methods / supplementary references
- GitHub, GitLab, Codeberg and public code references
- public documentation and API specifications
- university/research project pages
- public dashboards and GIS applications
- citizen-science and amateur sensor communities
- public cloud/open-data buckets and archives
- sitemaps and linked-data references
- links emitted by already discovered resources

Every newly found endpoint may itself be a catalog/root and must be fingerprinted and expanded.

## Geographic and temporal metadata

The catalog must support multiple scales. Never assume global or administrative geography is the primary view.

A resource may describe:

- point / station
- line / route / river / cable
- polygon / administrative area / watershed
- raster / tile coverage
- 3D/atmospheric/ocean volume
- satellite/orbital coverage
- network/topological scope
- no meaningful geographic scope

Temporal coverage and update cadence are independent fields. Preserve the ability to locate historical raw data later without requiring it to be continuously hot.

## Deduplication

The same resource may be discovered through many roots. Do not count discovery paths as independent resources.

Use reversible matching based on combinations of:

- canonical URLs and endpoint normalization
- persistent identifiers (DOI, ROR, repository IDs, etc.)
- publisher identifiers
- software/API fingerprints
- titles/descriptions
- spatial/temporal coverage
- content hashes when available
- explicit `sameAs`/upstream relationships

Keep all discovery/provenance paths even after deduplication.

## Discovery frontier

The census must expose what remains unresolved:

- uncrawled endpoints
- unexpanded catalogs
- unknown software/platform fingerprints
- unclassified resources
- referenced-but-undiscovered resources
- broken/dead endpoints
- auth/terms-blocked resources
- organizations with no checked data surfaces
- geographic/domain/language coverage gaps

The target is not a fake `100% complete` flag. The operational target is to continuously shrink and explain the frontier while discovering new frontier items.

## Phase 1 metrics

Track at minimum:

- organizations known / checked
- registries known / expanded
- catalogs known / expanded
- repositories known / expanded
- datasets/collections known
- APIs/feeds known
- sensor networks and sensors/instruments known
- resources by country, language and domain
- verified/broken/auth-required counts
- duplicate clusters and discovery paths
- new resources/day
- unresolved discovery frontier
- coverage disagreement against upstream registries

## First implementation milestones

1. Implement the canonical resource schema and append-only provenance.
2. Import several high-expansion root registries rather than individual datasets.
3. Normalize and deduplicate their catalog/repository/organization records.
4. Implement generic adapters for the highest-yield protocols/platforms.
5. Add an organization census and gap detector.
6. Add recursive child/reference expansion with rate limits and provenance.
7. Add verification/recheck scheduling.
8. Expose a search/API surface over the census.
9. Only then add multi-resolution globe/coverage visualization.

## Visualization principle

Do not render every resource as a point. The catalog can contain hundreds of millions of resources while the UI renders multi-resolution aggregates by space, time, domain, source type, access mode, and resolution. Zoom/filter operations progressively reveal organizations, catalogs, datasets, networks and individual sensors.

## Future relationship to intelligence/reconstruction

The Census deliberately comes first. Later systems can ask it questions such as:

`Which public resources can describe water conditions in this geometry during 2007-2011?`

and receive machine-readable candidates plus provenance and access instructions. Intelligence, correlation, reconstruction and AI reasoning are downstream consumers; they must not be prerequisites for catalog discovery.
