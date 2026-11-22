---
icon: lucide/box
---

# Chunk Generators

These are alternative formats for the `generator` field present *within* the [Dimension definition](https://minecraft.wiki/w/Dimension_definition#Generator_types). This is not a new registry, you must edit files with the `dimension` registry. 

### Formats

### Image Map

This generator is a variant of `minecraft:multi_noise` that will modify the `final_density` and `preliminary_surface_level` density functions in the provided noise settings' noise router with its own density functions that read from the heightmap to determine noise values. The `preliminary_surface_level` will be fully replaced, and the `final_density` will use the minimum density value of the heightmap and the original final density function.


- `{}` **generator**: The parent tag in a custom dimension. 
    - `"` **type**: Must be set to `novoatlas:image_map` for this generator to apply. 
    - `"` **map_info**: A resource location of the [map info](./map-info.md) object to use. 
    - `"` **settings**: A resource location of the [noise settings](https://minecraft.wiki/w/Noise_settings) to use. Usually, you will want this to be something like `minecraft:overworld`. 
    - `"` **underground_density_function**: A resource location of a [density function](https://minecraft.wiki/w/Density_function) to use. Can be any density function, but using one of `novoatlas:caves` or `novoatlas:no_caves` is recommended. 
    - `{}` **biome_source**: The biome source to use. Can be any biome source, but using the [color map biome source](./biome-sources.md) is highly recommended.
    - `T/F` **enable_carvers**: Optional boolean, defaults to `true`. A simple toggle for whether to disable *all* [carvers](https://minecraft.wiki/w/Cave#Carver_caves) (the old type of caves).


### Blend Image To Random

!!! warning
    This object is considered experimental, meaning its format could change in future updates. As such, it uses the namespace `novoatlas-experimental`!

This works similarly to [`novoatlas:image_map`](#image-map), except that outside the bounds of the map it will blend the image's height values to the random values provided by the **settings** field. The terrain inside of the map will match exactly as the image prescribes it to be, only the blocks outside of the map boundaries will be blended.

- `{}` **generator**: The parent tag in a custom dimension. 
    - `"` **type**: Must be set to `novoatlas-experimental:blend_image_map_to_random` for this generator to apply. 
    - `"` **map_info**: A resource location of the [map info](./map-info.md) object to use. 
    - `"` **settings**: A resource location of the [noise settings](https://minecraft.wiki/w/Noise_settings) to use. Usually, you will want this to be something like `minecraft:overworld`. 
    - `"` **underground_density_function**: A resource location of a [density function](https://minecraft.wiki/w/Density_function) to use. Can be any density function, but using one of `novoatlas:caves` or `novoatlas:no_caves` is recommended. 
    - `{}` **biome_source**: The biome source to use. Can be any biome source, but using the [color map biome source](./biome-sources.md) is highly recommended.
    - `T/F` **enable_carvers**: Optional boolean, defaults to `true`. A simple toggle for whether to disable *all* [carvers](https://minecraft.wiki/w/Cave#Carver_caves) (the old type of caves).
    - `F32` **blend_distance**: Optional 32-bit float, defaults to `64.0`. Must be greater than `0`. This is the distance over which the blending will apply in blocks. 

## Examples

A basic dimension definition to override the vanilla Overworld.

```json title="avila-basic-example/data/minecraft/dimension/overworld.json"
{
    "type": "minecraft:overworld",
    "generator": { // (1)!
        "type": "novoatlas:image_map",
        "map_info": "avila-example:avila",
        "settings": "minecraft:overworld",
        "underground_density_function": "novoatlas:caves",
        "biome_source": { // (2)!
            "type": "novoatlas:biome_cell_color_map",
            "map_info": "avila-example:avila",
            "default_biome": "minecraft:the_void"
        }
    }
}
```

1. This is the parent tag for the chunk generator.
2. This can be any [biome source](./biome-sources.md).

A custom dimension that blends the image-based, prescribed terrain to a random overworld-like terrain.

```json title="avila-blend-to-random-example/data/avila-random-example/dimension/avila.json"
{
    "type": "minecraft:overworld",
    "generator": {
        "type": "novoatlas-experimental:blend_image_map_to_random",
        "map_info": "avila-random-example:avila",
        "settings": "minecraft:overworld",
        "underground_density_function": "novoatlas:caves",
        "blend_distance": 256,
        "biome_source": {
            "type": "novoatlas-experimental:bounded_biome_cell_color_map",
            "map_info": "avila-random-example:avila",
            "default_biome": "minecraft:the_void",
            "blend_distance": 64,
            "outside_map": {
                "type": "minecraft:multi_noise",
                "preset": "minecraft:overworld"
            }
        }
    }
}
```
