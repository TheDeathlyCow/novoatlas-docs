---
icon: lucide/box
---

# Biome Sources

These are alternative formats for the `biome_source` field present *within* the [chunk generator](./chunk-generators.md) field of the [dimension definition](https://minecraft.wiki/w/Dimension_definition#Biome_sources). This is not a new registry, you must edit files with the `dimension` registry. 

## Formats

### Biome Cell Color Map

This biome source maps each pixel of your biome map images to a biome *cell*, which is a 4x4 section of blocks. Therefore, each pixel in your image will correspond to 16 blocks square.

- `{}` **biome_source**: The parent tag in the novoatlas chunk generator. 
    - `"` **type**: Must be set to `novoatlas:biome_cell_color_map` for this biome source to apply.
    - `"` **map_info**: A resource location of the map info object to use.
    - `"` **default_biome**: The biome to use for unmapped colors.

### Bounded Map Biome Cell Color Map

!!! warning
    This object is considered experimental, meaning its format could change in future updates. As such, it uses the namespace `novoatlas-experimental`!

This biome source works like [biome cell source](#biome-cell-color-map), except that it allows for a secondary generator to apply outside the bounds of its image and attempts to blend between them as best it can. The biome cells inside of the map will match exactly as the image prescribes it to be, only the cells outside of the map boundaries will be blended.

- `{}` **biome_source**: The parent tag in the novoatlas chunk generator. 
    - `"` **type**: Must be set to `novoatlas-experimental:bounded_biome_cell_color_map` for this biome source to apply.
    - `"` **map_info**: A resource location of the map info object to use.
    - `"` **default_biome**: The biome to use for unmapped colors.
    - `{}` **outside_map**: Another biome source, either a NovoAtlas biome source or a vanilla one, to be used outside the bounds of the map.
    - `F32` **blend_distance**: A 32-bit float. Must be greater than `0`. How far, in biome cells, to apply blending between the inside and outside map.
    - `"` `{}` **normal_noise**: Optional [noise function](https://minecraft.wiki/w/Noise) applied to the blending. Defaults to being equivalent to `minecraft:offset`. 

### Color Map
!!! warning
    This generator is deprecated, and has been replaced by the [biome cell source](#biome-cell-color-map).

This biome source maps each pixel of your biome map images to a single block. Therefore, each pixel in your image will correspond to 1 block. Because Minecraft actually uses cells to place biomes, this will cause almost all pixels in the image to be unused. For this reason it is highly recommended to use the [biome cell source](#biome-cell-color-map) instead.

- `{}` **biome_source**: The parent tag in the novoatlas chunk generator. 
    - `"` **type**: Must be set to `novoatlas:color_map` for this biome source to apply.
    - `"` **map_info**: A resource location of the map info object to use.
    - `"` **default_biome**: The biome to use for unmapped colors.


## Examples

See the biome sources in the [Chunk generator examples](./chunk-generators.md#examples).