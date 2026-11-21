---
icon: lucide/map
---

# Map Info

This is the central registry of NovoAtlas. It is responsible for managing all the images for your world, basically everything comes through here.

!!! tip
    All Map Info entries go in the `novoatlas/map_info` registry in your datapack. For example, `data/my-awesome-datapack/novoatlas/map_info/my_awesome_world.json`.

- `{}`: The root tag.
    - `"` **height_map**: The resource location of the height map to use, must be a black and white image in the registry `novoatlas/heightmap`. 
    - `"` **fluid_height_map**: Optional. The resource location of the fluid height map to use, must be a black and white image in the registry `novoatlas/heightmap`. If not specified, the world's regular sea level will be used.
    - `I32` **starting_y**: Tells the game how high the lowest block in the heightmap should be. I.e., the elevation that a black pixel on the heightmap represents in-game.
    - `[]` **center_offset**: An optional list of 2 floats that offsets the center position of the map. The units are in `[width, height]` respectively, NOT in blocks. Defaults to `[0, 0]`. 
    - `"` **image_wrapping**: Optional enumerated string that specifies a mode for how to wrap the map image beyond the borders of the central image. Valid values are `clamp_to_edge`, `repeat`, and `mirrored_repeat`. Defaults to `clamp_to_edge`.
    - `{}` **surface_biomes**: A biome color provider containing information about the surface biomes. See below for format.
    - `{}` **cave_biomes**: An optional compound containing information about the cave biomes. If left empty, no cave biomes will generate.
        - `[]` **layers**: A list of [biome layer entries](#biome-layer-entry) that describes what cave biomes to generate (roughly) at each y-level.
    - `I32` **surface_range**: Optional integer, defaults to `16`. The depth below the surface of the heightmap that cave biomes should begin spawning, if any cave biomes are specified.
    - `{}` **scaling**: A scale configuration compound
        - `F32` **vertical_scale**: An optional positive float that multiplies the heightmap values, allowing for the height to be increased or decreased. Useful for 16-bits-per-color images. Defaults to `1.0`.
        - `F32` **horizontal_scale**: An optional positive float that multiplies the size of the map. When specified as a float, will use `nearest_neighbor` interpolation. Defaults to `1.0`.
        - `{}` **horizontal_scale**: The horizontal scale can alternatively be specified as an optional compound, allowing for configuration of the interpolation strategy.
            - `F32` **value**: A positive float that multiplies the size of the map.
            - `"` **interpolation**: A resource location identifying the interpolation type to use, to allow for smooth blending between pixel values (especially with large `value`s). Must be one of the [Interpolation Types](#interpolation-types). Defaults to `novoatlas:nearest_neighbor`. 
            - Additional fields, depending on the value of `interpolation`.

!!! note
    The cave biomes layers list will only use the first entry in this list to match a given y value. 

### Biome Layer Entry

- `{}`: The root tag.
    - `{}` **y_range**: The range at which to place this layer.
        - `I32` **min**: Optional integer, the minimum y-range to place this layer at. Must be strictly less than **max**.
        - `I32` **max**: Optional integer, the maximum y-range to place this layer at. 
    - `{}` **biomes**: A [biome color provider](#biome-color-provider) containing information about this layer's biomes. See below for format.

!!! tip
    Having a layer use the special biome type `novoatlas:surface_biome` in its color provider will cause the generator to place the surface biome at that (x, z) position in the layer. 

### Biome Color Provider

- `{}`: The root tag. 
  - `"` **map**: The resource location of the biome map to use, must be a colour image in the registry `novoatlas/biome_map`.
  - `T/F` **strict**: Optional boolean. Whether to be strict in the mapping of colours to biomes. If true, then the default biome of the parent biome source will be used when a colour that is not defined in the **biomes** list is sampled in the map. Otherwise, the closest colour (based on RGB vector distance) is used instead. Defaults to `false`.
  - `[]` **biomes**: A list that maps colors in the biome map image to actual biomes. Each color may ONLY APPEAR ONCE in this list! List entries are of the following format:
    - `{}`: The root tag.
      - `"` **biome**: The resource location of the biome this color maps to.  
      - `I32` `"`: An 8-bit RGB color value, may be either an integer or a string. If an unquoted integer, then must be a decimal representation of the RGB color value. If a quoted string, then it must be a `#`-prefixed RGB color hex code. More formally, this string must match the regular expression `#[0-9a-fA-F]{6}`. That means a string must be of the form `#XXXXXX` where `X` is a hexadecimal digit.

### Interpolation Types

NovoAtlas provides a registry for interpolation types. Third-party mods can register new codecs to this registry to provide new values, however NovoAtlas provides a few built in. You may choose any one of the following types:

- `{}` **horizontal_scale**: The root tag.
    - `"` **interpolation**: If `novoatlas:nearest_neighbor` or `minecraft:nearest_neighbor` or `nearest_neighbor`, will use a simple blocky interpolation. This is the default strategy. Has no additional fields.

- `{}` **horizontal_scale**: The root tag.
    - `"` **interpolation**: If `novoatlas:bilinear` or `minecraft:bilinear` or `bilinear`, will use a linearly interpolated smoothing that eliminates blockiness for large scaling values but is still somewhat jagged. Has no additional fields.

- `{}` **horizontal_scale**: The root tag.
    - `"` **interpolation**: If `novoatlas:bicubic` or `minecraft:bicubic` or `bicubic`, will use a cubic interpolated smoothing that is less jagged than bilinear but has worse performance and will still produce noticeable artefacts at large scaling values. Has no additional fields.

- `{}` **horizontal_scale**: The root tag.
    - `"` **interpolation**: If `novoatlas:lanczos` or `minecraft:lanczos` or `lanczos`, will use [Lánczos interpolated](https://mazzo.li/posts/lanczos.html) smoothing that produces better results than bilinear and bicubic for very large scaling values but has generally the same or much worse performance, depending on the size of the `window`. 
    - `I32` **window**: Optional positive 32-bit integer. Specifies the window size of the interpolation. Higher values may produce better, but very quickly increases CPU load. Defaults to `2`.

## Examples

A simple map info from the Avila Basic Example datapack.

```json title="data/avila-example/novoatlas/map_info/avila.json"
{
    "starting_y": 0,
    "scaling": {
        "horizontal_scale": {
            "value": 1,
            "interpolation": "nearest_neighbor"
        },
        "vertical_scale": 1
    },
    "height_map": "avila-example:avila",
    "center_offset": [0.0, 0.0],
    "image_wrapping": "clamp_to_edge",
    "surface_biomes": {
        "map": "avila-example:avila",
        "strict": false,
        "biomes": [
            {
                "biome": "minecraft:plains",
                "color": "#6D923D"
            },
            {
                "biome": "minecraft:beach",
                "color": "#C4B058"
            },
            {
                "biome": "minecraft:jungle",
                "color": "#338C2A"
            },
            {
                "biome": "minecraft:sparse_jungle",
                "color": "#738533"
            },
            {
                "biome": "minecraft:desert",
                "color": "#AEA54A"
            },
            {
                "biome": "minecraft:jagged_peaks",
                "color": "#FFFFFF"
            },
            {
                "biome": "minecraft:meadow",
                "color": "#7FC458"
            },
            {
                "biome": "minecraft:grove",
                "color": "#8AAD88"
            },
            {
                "biome": "minecraft:old_growth_spruce_taiga",
                "color": "#224D20"
            },
            {
                "biome": "minecraft:stony_shore",
                "color": "#525252"
            },
            {
                "biome": "minecraft:ocean",
                "color": "#313A8B"
            },
            {
                "biome": "minecraft:warm_ocean",
                "color": "#474D90"
            },
            {
                "biome": "minecraft:cold_ocean",
                "color": "#19248A"
            },
            {
                "biome": "minecraft:deep_ocean",
                "color": "#10175E"
            },
            {
                "biome": "minecraft:stony_peaks",
                "color": "#939393"
            },
            {
                "biome": "minecraft:taiga",
                "color": "#338553"
            }
        ]
    }
}
```