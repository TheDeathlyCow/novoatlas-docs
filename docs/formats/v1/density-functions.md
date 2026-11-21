---
icon: lucide/box
---

# Density Functions

These are additional types of [density functions](https://minecraft.wiki/w/Density_function) supplied by NovoAtlas. These can be used in any `density_function` registry entry or in-line where density functions are required in other things like noise routers.

## Heightmap

Using a heightmap in a [map info](./map-info.md), computes positive density below the surface and negative density above the surface, with some blending in between.

- `{}`: The parent tag.
    - `"` **type**: Must be set to `novoatlas:heightmap` for this density function to apply. 
    - `"` **map_info**: A resource location of the [map info](./map-info.md) object to use for the heightmap data. 
    - `F32` **transition_range**: Optional 32 bit float, defaults to `10`. Specifies how wide the density values should interpolate above/below the surface.

## Get Height From Map

Returns the raw height value from a heightmap in a [map info](./map-info.md).

- `{}`: The parent tag.
    - `"` **type**: Must be set to `novoatlas:get_height_from_map` for this density function to apply. 
    - `"` **map_info**: A resource location of the [map info](./map-info.md) object to use for the heightmap data. 
    - `F32` **lower_bound**: A 32 bit float between -2032 and 2031 (inclusive). Clamps the minimum height to this value.
    - `F32` **upper_bound**: A 32 bit float between -2032 and 2031 (inclusive). Clamps the maximum height to this value.

## Get Preliminary Height From Map

Does the exact same thing like as [Get Height From Map](#get-height-from-map), but subtracts the height value by `8` to allow it to be used as a preliminary height in the noise router.

- `{}`: The parent tag.
    - `"` **type**: Must be set to `novoatlas:get_height_from_map` for this density function to apply. 
    - `"` **map_info**: A resource location of the [map info](./map-info.md) object to use for the heightmap data. 
    - `F32` **lower_bound**: A 32 bit float between -2032 and 2031 (inclusive). Clamps the minimum height to this value.
    - `F32` **upper_bound**: A 32 bit float between -2032 and 2031 (inclusive). Clamps the maximum height to this value.

## Blend At Map Border

!!! warning
    This object is considered experimental, meaning its format could change in future updates. As such, it uses the namespace `novoatlas-experimental`!

This density function is used to blend two other density functions together based on the boundaries of a map. It is intended to be used as the final density for blend-to-random generation. Around the boundary of the map, it will blend the edge of the map to the random density by using a blended preliminary height value to generate densities similar to [`novoatlas:height_map`](#heightmap).

- `{}`: The parent tag.
    - `"` **type**: Must be set to `novoatlas-experimental:blend_at_map_border` for this density function to apply. 
    - `"` **map_info**: A resource location of the [map info](./map-info.md) object to use to determine the boundaries of the map.
    - `"` `{}` **inside_map**: The density function to use when generating inside the map. This should supply the final density for blocks inside the map.
    - `"` `{}` **outside_map**: The density function to use when generating outside the map. This should supply the final density for blocks outside the map.
    - `"` `{}` **preliminary_height**: A density function that gives the blended preliminary height values between the **outside_map** and **outside_map** density functions. This should always be done with [`novoatlas:blend_surface_level`](#blend-surface-level). This function type will add `8` to the output of this function.
    - `F32` **blend_distance**: An 32-bit float. Must be greater than `0`. How far, in blocks, to blend the two inside/out map functions using the preliminary surface level.

## Blend Surface Level

!!! warning
    This object is considered experimental, meaning its format could change in future updates. As such, it uses the namespace `novoatlas-experimental`!

This density function will use smoothstep to blend together the outputs of two other density functions based on the boundaries of a map.

- `{}`: The parent tag.
    - `"` **type**: Must be set to `novoatlas-experimental:blend_surface_level` for this density function to apply. 
    - `"` **map_info**: A resource location of the [map info](./map-info.md) object to use to determine the boundaries of the map.
    - `"` `{}` **inside_map**: The density function to use when generating inside the map. This should supply the preliminary height values for blocks inside the map.
    - `"` `{}` **outside_map**: The density function to use when generating outside the map. This should supply the preliminary height values for blocks outside the map.
    - `F32` **blend_distance**: An 32-bit float. Must be greater than `0`. How far, in blocks, to blend the two inside/out map functions using the preliminary surface level.