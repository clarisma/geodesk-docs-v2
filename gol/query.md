---
layout: default
title: query
parent: GOL Utility
nav-order: 6
has_toc: false
permalink: /gol/query
---

# `gol query`

Prints all features that match the given query to `stdout`, in a variety of formats.

Usage:

    gol query [<options>] <gol-file> <query>

The query must be written in [GOQL](../goql), the Geo-Object Query Language. GOQL is similar to [MapCSS](https://wiki.openstreetmap.org/wiki/MapCSS/0.2), which is used by Mapnik and Overpass to select OSM objects.

For example:

    gol query geodata/france 
      na[amenity=fire_station], n[emergency=fire_hydrant]
      -b 2.2,48.8,2.5,48.9 -f geojson 

retrieves all fire stations (which can be nodes or areas) and hydrants
(which only exist as nodes) from the `france.gol` library (stored in the `geodata`
folder). The features must lie fully or partially inside the specified bounding box (a rectangle that covers metropolitan Paris) and are printed to `stdout` as GeoJSON (*see below*).

## Options

{% include gol/option-area.md %}
{% include gol/option-bbox.md %}

### `-f`, `--format` <code><em>&lt;TYPE&gt;</em></code> {#option-format}

The [output format](#output-formats) of the results:

`brief` | Prints a per-feature overview (default) 
`count` | Prints only the *number* of features. 
`csv` | Comma-separated values
`geojson` / `geojsonl` | GeoJSON ([traditional](https://geojson.org) / [one feature per line](https://stevage.github.io/ndgeojson/))
`list` | Simple list of IDs (*default*)
`table` | Simple text-based table
`wkt` | Well-known text (geometries only)
`xml` | [OSM-XML](https://wiki.openstreetmap.org/wiki/OSM_XML)


### `-k`, `--keys` <code><em>&lt;LIST&gt;</em></code> {#option-keys}

A comma-separated list of OSM keys, which determine the feature tags to be
included in the output.

There are several special keys that only apply to the `csv` output option and produce additional columns:

<table>
  <tr>
    <td><code>bbox</code></td>
    <td markdown="span">

The bounding-box coordinates of the feature (West, South, East, North).
For `--format=geojson`, this becomes the [`bbox`](https://datatracker.ietf.org/doc/html/rfc7946#section-5) member of the `Feature` object.
 
  </td>
</tr>
<tr>
  <td><code>geom</code></td>
  <td>

The geometry of the feature (as <a href="https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry">well-known text</a>).<br>
          Only applies to <code>--format=csv</code>. 
      </td>
    </tr>
    <tr>
      <td><code>lat</code></td>
      <td>
          The WGS-84 latitude of the feature's <em>center</em> (see <code>--center</code>). 
      </td>
    </tr>
    <tr>
      <td><code>lon</code></td>
      <td>
          The WGS-84 longitude of the feature's <em>center</em> (see <code>--center</code>). 
      </td>
    </tr>
    <tr>
      <td><code>tags</code><br>(or&nbsp;<code>*</code>)</td>
      <td>
          A comma-separated list of key-value pairs, representing the keys of the tags
          that are not already printed in a column of their own. 
       </td>
    </tr>
    <tr>
      <td><code>x</code></td>
      <td>
          The Mercator-projected X coordinate of the feature's <em>center</em> (see <code>--center</code>). 
      </td>
    </tr>
    <tr>
      <td><code>y</code></td>
      <td>
          The Mercator-projected Y coordinate of the feature's <em>center</em> (see <code>--center</code>). 
      </td>
    </tr>
  </table>

  If `--tags` is omitted, all tags are included in the output.

  `--tags` is ignored if the requested output format is `count` or `list`.


{% include gol/option-output.md %}

### `-p`, `--precision` <code><em>&lt;NUMBER&gt;</em></code> {#option-precision}

Value: 0 -- 15 (default: 7)

The coordinate precision (digits after the decimal point) to use.

<!--
<div class="language-plaintext highlighter-rouge">
<pre class="highlight"><code>--precision=<em>0-15</em></code></pre></div>
-->

Applies only to `csv`, `geojson`/`geojsonl` and `wkt`.


{% comment %}

## Output Formats

Specified by the [`--format`](#option-format) option.

### `-f=csv` {#format-csv}

Outputs features in table-based form, suitable for import into a spreadsheet
application or an SQL database.

The first row contains the header, the following rows each represent a single
feature. Use in conjunction with `--tags` to specify which tags
should be written.

Columns are separated by a tab character. The first two columns are always
`t` (the OSM type of the element: `N`, `W`, or `R`) and `id` (the OSM id
of the element).

If the `--tags` option is not specified, the only other column is `tags`,
which includes all the features' tags as a comma-separated
lists of key-value pairs.

Supported options: `f:id`, `f:sort`

### `-f=poly` {#format-poly}

Outputs features in [polygon-file format](https://wiki.openstreetmap.org/wiki/Osmosis/Polygon_Filter_File_Format). Non-polygonal features are omitted.
The resulting file can be used for the `--area` option in subsequent queries.

{% endcomment %}

