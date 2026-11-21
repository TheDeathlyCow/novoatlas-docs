---
icon: lucide/rocket
---

# Getting Started

## How to Create a NovoAtlas Dimension

1. Create or find a grayscale image height map in one of the supported formats[^1]. You may either use 8-bit RGB or 16-bit grayscale, if your image format supports it. To create maps, I would highly recommend using [WorldPainter](https://www.worldpainter.net/), as it is purpose-built for Minecraft map making and has an option to export a height map. You can also get heightmaps of Earth from a variety of websites, here's a few that I've found (though not tested with NovoAtlas):
    * https://tangrams.github.io/heightmapper/
    * https://visibleearth.nasa.gov/images/73934/topography
    * https://heightmap.skydark.pl/ (Requires a Mapbox API key)
2. Create a copy of the map and paint over it with whatever colours you like. Each colour corresponds to a different biome. Do not mix or blend the colours. Save this image as one of the supported formats[^1] with 8-bit RGBA encoding. For cave biomes, you can create separate maps for each "layer" of the world.
3. Create a datapack and add these images to it, you can either copy one of the example datapacks or make one from scratch yourself.

---

[^1]: Supported formats include: `.png`, `.jpg`,`.jpeg`,  `.tif`, `.tiff`, `.wbp`, `.bmp`, `.gif`, `.wbmp`, and `.webp`. Some of these formats support lossy compression, however I strongly recommend against utilising this feature, especially for biome maps. Using lossy compression will result in loss of detail and incorrect biome placement (particularly around biome boundaries).

## Examples

The Example Datapacks are builtin with NovoAtlas and can be selected on world creation. You can also download them separately from the [GitHub releases page](https://github.com/TheDeathlyCow/novoatlas/releases). 

## Modifying a NovoAtlas Dimension

!!! note
    Modifying dimensions requires that you already know how to configure datapacks. If you have never done this before, the [Minecraft Wiki](https://minecraft.wiki/w/Tutorial:Creating_a_data_pack) has a basic overview (there are also tutorials on YouTube).

You can of course, create or modify a NovoAtlas by yourself too. This is mainly done through the new datapack registry `novoatlas/map_info`. The full format of map info objects can be found [here](./formats/v1/map-info.md). The height map and biome map images have separate registries, `novoatlas/heightmap` and `novoatlas/biome_map` respectively. 

Once you have your map info, you will need to create or modify a dimension. The actual generation of the map is done with a custom chunk generator ([format](./formats/v1/chunk-generators.md)) and custom biome source ([format](./formats/v1/biome-sources.md)).

## Some Tips

- The easiest way to create a NovoAtlas datapack is to copy an example back from the [releases page](https://www.github.com/TheDeathlyCow/novoatlas/releases) of NovoAtlas and replace the height and biome maps with your own. Your biome map should be 1/4 the width and height of your heightmap for them to match.
- When you're drawing the biomes, it may be helpful to be able to see the heightmap as a contour map. You can emulate this in your preferred photo editor by selecting a black pixel and then using "select by colour" with varying thresholds to get varying contour lines of your map.
- Your map will be centred at `(0, 0)`, unless you supply an offset. Up in the image is north. If your map is an uneven number of pixels, the last pixel on the south and/or east side will be cut off.
- If you use 16-bit-per-color heightmap images to allow for heightmap values outside the 256 block range[^2], and you are getting a flat world at y=320, try using the [vertical scale option in the map info format](./formats/v1/map-info.md) to reduce the final heights of the map to more sensible Minecraft values. For example, in maps that use the full 16-bit range, a vertical scale of `320 / 65535 = 0.004882887` works well.
- When creating large maps, you can use the `horizontal_scale` option to increase the size of the world from smaller images, which will save RAM and disk space. The default interpolation strategy will produce rather blocky results, but you can also try using other interpolation types as specified in the [map info format](./formats/v1/map-info.md#interpolation-types).
- To allow for lakes and rivers at varying heights from sea level, use the `fluid_height_map` option of the map info. This is a separate map to your regular terrain that tells the game what level to generate the dimension's default fluid at (normally water). If two different fluid levels meet and there is not land to separate them, then the game will try to generate barriers between them, like aquifers do.
- The mod [Worldgen Devtools](https://modrinth.com/mod/worldgen-devtools) can be helpful for advanced development and debugging.

[^2]: Like [this one](https://www.moddb.com/addons/map-of-middle-earth-textures).