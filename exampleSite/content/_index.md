---
title: "Hugo Maps"
date: 2020-05-01T00:00:00+01:00
draft: false
---

# Beautiful Maps for Hugo

`hugo-maps` is a [Hugo](https://gohugo.io/) module that allows you to easily add beautiful maps to your Hugo site. It uses [MapLibre GL JS](https://docs.mapbox.com/mapbox-gl-js/api/) to render vector maps.

You can either use a commercial map server (e.g., [Maptiler](https://www.maptiler.com/)) or a self-hosted map server (e.g., [Tileserver GL](https://github.com/maptiler/tileserver-gl)) to generate the vector map tiles.

You can [**go to the examples**](#examples) or [**look at the included themes**](#themes). 

## Features
- 🌍 High-quality interactive vector maps.
- 🎨 Comes with lots of styles. You can also use your own custom style if you want.
- 🅰️ Glyphs for all included styles are included in the module.
- 📍 Support for custom markers.
- 💾 Supports download of vector tiles at deploy time (no external API calls at runtime).

**{{< hint >}} Local maps are currently not supported, pending minor changes to Hugo's template engine. {{< /hint >}}**

## Getting Started

### 1. Add the module to your site

Add the following to your site's `config` file:

```yaml
module:
  imports:
    - path: github.com/marcpabst/hugo-maps
```

### 2. Add a tile source

You need to add a tile source to your site's `config` file. You can either use a commercial tile server (e.g., [Maptiler](https://www.maptiler.com/)) or a self-hosted tile server. If you want to use Maptiler, you need to [register for an account](https://www.maptiler.com/cloud/) and get an API key.

Add the URL of the tile server to your site's `config` file (you can also do this on a per-map basis):

```yaml
params:
  hugo-maps:
      default:
        tileJSON: https://api.maptiler.com/tiles/v3/tiles.json?key={key}
```

### 3. Define a map and add it to your site

You can define a map in your site's `config` file. The following example defines a map named `my_map`:

```yaml
params:
  hugo-maps:
    default:
        tileJSON: https://api.maptiler.com/tiles/v3/tiles.json?key={key}
    my_map:
        center: [0, 0]
        zoom: 2
```

You can add a map to your site by using the `map` shortcode and the `name` argument:

````html
{{</* map name="my_map" */>}}
````

Optionally, you can override the default configuration for the map by passing additional arguments to the shortcode. For example, you can change the height of the map by passing a `height` argument.

````html
{{</* map name="my_map" height="350px" */>}}
````


## Configuration

Maps are configured using the `hugo-maps` parameter in your site's `config` file. A map is defined by a unique name and a configuration. The `default` configuration is used for all maps unless you override it for a specific map.
    
```yaml
params:
    hugo-maps:
        default:
            ... default configuration ...
        my_map:
            ... configuration for "my_map" ...
```

Here are the available configuration options with their default values:

```yaml
type: remote # type of map - remote or local
style: osm-bright # theme to use
tilesMinZoom: # minimum zoom level for local maps
tilesMaxZoom: # maximum zoom level for local maps
center: [-0.127758, 51.507351] # center of the map
minZoom: 0 # minimum zoom level
maxZoom: 23 # maximum zoom level
zoom: 11 # initial zoom level
minPitch: 0 # minimum pitch
maxPitch: 60 # maximum pitch
pitch: 0 # initial pitch
bearing: 0 # initial bearing
antialias: false # whether to use antialiasing
width: 100% # width of the map
height: 300px # height of the map
attributionControl: true # whether to show attribution
customAttribution: '<a href="https://www.maptiler.com/copyright/" target="_blank">© MapTiler</a> <a href="https://www.openstreetmap.org/copyright" target="_blank">© OpenStreetMap contributors</a>' # custom attribution
interactive: true # whether the map is interactive
```

## Examples

All examples use the following `default` configuration. You can override this configuration for each map. Please note that to use Maptiler's tile sever, you need to [register for an account](https://www.maptiler.com/cloud/) and get an API key.

```yaml
params:
  hugo-maps:
      default:
        tiles: https://api.maptiler.com/tiles/v3/{z}/{x}/{y}.pbf?key={key}

```

### Example 1: Custom map center

```yaml
params:
  hugo-maps:
    new_york_map:
        center: [-74.005833, 40.712778]
```

{{< map name="new_york_map" >}}

### Example 2: Styling the map
```yaml
params:
  hugo-maps:
    my_london_map:
        style: toner
        center: [-0.127758, 51.507351]
```

{{< map name="map2" >}}

### Example 3: 3D map with marker

```yaml
params:
  hugo-maps:
    my_london_map:
        type: local
        style: maptiler-3d
        center: [-0.127758, 51.507351]
        tilesBoundingBox: [0.49956765, 0.3325506204915718, 0.4997617583333333, 0.3323975306107756]
        tilesMinZoom: 0
        tilesMaxZoom: 15
        minZoom: 4
        maxZoom: 24
        zoom: 14
        pitch: 45
        antialias: true
        markers:
          - location: [-0.127758, 51.507351]
            text: 'Welcome to London'
```

{{< map name="map_3d" >}}


## Themes

### OSM-Bright (`osm-bright`)

{{< map name="theme_map" style="osm-bright" height="200px" >}}

### MapTiler Basic (`maptiler-basic`)

{{< map name="theme_map" style="maptiler-basic" height="200px" >}}

### MapTiler 3D (`maptiler-3d`)

{{< map name="theme_map" style="maptiler-3d" height="200px" >}}

### Toner (`toner`)

{{< map name="theme_map" style="toner" height="200px" >}}

### Positron (`positron`)

{{< map name="theme_map" style="positron" height="200px" >}}

### Fjord Color (`fjord-color`)

{{< map name="theme_map" style="fjord-color" height="200px" >}}

### Darkmatter (`darkmatter`)

{{< map name="theme_map" style="darkmatter" height="200px" >}}

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details. OpenMapTiles styles are licensed under the BSD 3-Clause License. Please make sure that you comply with the license of the map tiles you use.