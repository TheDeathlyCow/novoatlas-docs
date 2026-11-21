---
icon: lucide/brush
---

# Images

In NovoAtlas, all images are supplied via datapacks in a one of two custom registries: `novoatlas/heightmap` or `novoatlas/biome_map`. Heightmaps can be any grayscale image, and it supports both 8-bit and 16-bit grayscale PNG images. Biome maps can be any 8 bit-per-channel RGB image. 

You may supply images as `.png`, `.jpg`,`.jpeg`,  `.tif`, `.tiff`, `.wbp`, `.bmp`, `.gif`, `.wbmp`, or `.webp`. While some of these formats support lossy compression, this will almost certainly lead to issues in NovoAtlas. Therefore, it is highly recommended that users stick to `.png` where possible, or disable all lossy compression otherwise. These images are fully decompressed when loaded into memory, so you would not gain much from having high lossy compression anyway.

Also, if two images in the same registry have the same path/name, but different formats, then one will override the other. For example, you cannot have `novoatlas/heightmap/avila.png` and `novoatlas/heightmap/avila.webp` at the same time; you must pick one.

