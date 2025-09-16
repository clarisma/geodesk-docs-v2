---
layout: default
title: load
parent: GOL Utility
nav-order: 5
---

# `load`

Imports tiles into a Geo-Object Library from a Geo-Object Bundle.

Usage:

    gol load <gol-file> <gob-file>] [<options>]

If no area is defined (via [`--area`](#option-area) or [`--bbox`](#option-bbox)), all tiles that aren't already present in the Library are imported from the Bundle.

The Library and the Bundle must have the same tileset ID.

## Options

{% include gol/option-area.md %}
{% include gol/option-bbox.md %}
{% include gol/option-quiet.md %}
{% include gol/option-silent.md %}
{% include gol/option-verbose.md %}
{% include gol/option-wait.md %}

