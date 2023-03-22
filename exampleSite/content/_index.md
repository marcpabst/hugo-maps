---
title: "Hugo Maps"
date: 2020-05-01T00:00:00+01:00
draft: false
---

# Beautiful Maps for Hugo

`hugo-maps` is a [Hugo](https://gohugo.io/) module that allows you to easily add beautiful maps to your Hugo site. It uses [MapLibre GL JS](https://docs.mapbox.com/mapbox-gl-js/api/) to render vector maps.

You can either use a commercial map server (e.g., [Maptiler](https://www.maptiler.com/)) or a self-hosted map server (e.g., [Tileserver GL](https://github.com/maptiler/tileserver-gl)) to generate the vector map tiles.

## Features
- Built-in support for all [OpenMapTiles](https://openmaptiles.org/styles/) styles. You can also use your own custom style if you want.
- You can either download the map tiles during the build process and host them alongside your site (local maps) or use a remote map server (remote maps).
- Support for custom markers.

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


The following configuration options are available:

- `tiles`: The URL of the map tiles, e.g., "https://api.maptiler.com/tiles/v3/{z}/{x}/{y}.pbf?key={key}"
- `type`: `remote` or `local`. If `local`, the map tiles are downloaded during the build process and stored locally. If `remote`, the map tiles are downloaded at runtime.
- `style`: One of `toner`, `positron`, `maptiler-3d`, `maptiler-basic`
- `tilesMinZoom`: The minimum zoom level for the map tiles. This is usually 0.
- `tilesMaxZoom`: The maximum zoom level for the map tiles. Be careful when using a `local` map, as the maps can be quite large when using a high zoom level or a large area.
- `center`: The center of the map. This is a two-element array with the longitude and latitude of the center (e.g., `[0, 0]`).
- `minZoom`: The minimum zoom level for the map (minimum is 0).
- `maxZoom`: The maximum zoom level for the map (maximum is 24).
- `zoom`: The initial zoom level for the map.
- `minPitch`: The minimum pitch for the map (minimum is 0).
- `maxPitch`: The maximum pitch for the map (maximum is 60).
- `pitch`: The initial pitch for the map.
- `bearing`: The initial rotation for the map.
- `antialias`: Whether to use antialiasing for the map.

## Usage

You can add a map to your site by using the `map` shortcode. The shortcode takes a single argument, which is the name of the map you want to display.

````html
{{</* map "map_name" */>}}
````

Optionally, you can provide width and height parameters to the shortcode. If you don't provide these parameters, the map will be displayed at 100% width and 400px height.

````html
{{</* map "map_name" width="90%" height="350px" */>}}
````

## Examples

All examples use the following `default` configuration. You can override this configuration for each map. Please note that to use Maptiler's tile sever, you need to [register for an account](https://www.maptiler.com/cloud/) and get an API key.

```yaml
params:
  hugo-maps:
      default:
        tiles: https://api.maptiler.com/tiles/v3/{z}/{x}/{y}.pbf?key={key}
```

### Example 1: Simple remote map

```yaml
params:
  hugo-maps:
    my_london_map:
        type: remote
        center: [-0.127758, 51.507351]
```

{{< map "map1" >}}

### Example 2: Remote map with specific style

```yaml
params:
  hugo-maps:
    my_london_map:
        type: remote
        style: toner
        center: [-0.127758, 51.507351]
```

{{< map "map2" >}}

### Example 3: Local map with 3D theme and custom marker 

"Local" maps are maps that are not hosted on a remote server. They are downloaded at deploy time and stored locally in the `:cacheDir` folder. This is useful if you only need to cover a small area and don't want to use a remote server.

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

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details. OpenMapTiles styles are licensed under the BSD 3-Clause License. Please make sure that you comply with the license of the map tiles you use.