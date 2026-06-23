# Exoplanet Archive

> **In plain terms:** Programmatic access to NASA's catalog of confirmed planets and candidates outside our solar system. You write filter queries against the database — e.g., planets in the Kepler field or small, temperate candidates — aimed at research and planet-hunting analysis.

## Introduction

The Exoplanet Archive API allows programatic access to NASA's Exoplanet Archive database. This API contains a ton of options so to get started please visit this page for introductory materials. To see what data is available in this API visit here and also be sure to check out best-practices and troubleshooting in case you get stuck. Happy planet hunting!

## TAP service (used by the `exoplanet_query` tool)

The `exoplanet_query` tool targets the archive's **TAP** service, which takes an
ADQL (SQL-like) query and a format:

```
GET https://exoplanetarchive.ipac.caltech.edu/TAP/sync?query=<ADQL>&format=json
```

| Parameter | Description |
|-----------|-------------|
| `query`   | ADQL query string, e.g. `select pl_name, hostname, disc_year from ps where disc_year > 2020` |
| `format`  | `json` (default), `csv`, or `votable` |

Common tables: `ps` (planetary systems), `pscomppars` (composite parameters),
`cumulative` (Kepler Objects of Interest). NASA's TAP endpoint accepts the bare
`query`/`format` pair — `REQUEST=doQuery` and `LANG=ADQL` are not required.

### Example ADQL queries

| Goal | ADQL |
|------|------|
| Planets discovered after 2020 | `select pl_name, hostname, disc_year from ps where disc_year > 2020` |
| Confirmed planets that transit their host stars | `select pl_name, hostname from ps where tran_flag = 1` |
| Small, temperate KOI candidates | `select kepoi_name, koi_prad, koi_teq from cumulative where koi_prad < 2 and koi_teq between 180 and 303 and koi_disposition = 'CANDIDATE'` |

## Legacy interface (nph-nstedAPI)

An older interface is still reachable for some tables; it uses `table`/`where`/`format`
query parameters rather than ADQL. The `exoplanet_query` tool does **not** use this:

| Example API | URL |
|-------------|-----|
| Confirmed planets in the Kepler field | `https://exoplanetarchive.ipac.caltech.edu/cgi-bin/nstedAPI/nph-nstedAPI?&table=exoplanets&format=ipac&where=pl_kepflag=1` |
