---
layout: lightbake
permalink: /light-bake/faq
meta_description: >
  LightBake is a light baking solution for Blender. It allows you to bake light for real-time rendering engines and comprises two key tools: a lightmap baking system and a probes baking system.

meta_keywords: Blender, Plugin, 3D, GI, Baking, FAQ
section: lightbake-faq
---

# Light bake FAQ

### I installed the plugin, but I can't bake anything?

First, check the output property panel to see if there is a Light bake sub-panel. If it's not there, the plugin may not be installed correctly. Verify in the Blender preferences if the plugin is enabled.

If the panel is present, you need to setup the export to specify where the plugin should render maps and export data; then, all other options will become available.

### I've baked my lightmaps. Where are they, and what do I do now?

The lightmaps are in the export directory, as defined in Output > Light bake property panel.

They are now ready to be used in your favorite real-time rendering engine. Confirm that you've exported the data in the correct place, as explained in the next question. You can preview lightmaps by using them as textures in Blender.

### I can't preview my lightmaps in Blender

Currently, the plugin does not support previewing lightmaps in Blender. The plugin exports lightmaps as images in the textures folder (see export settings in the documentation) and can be used as textures in Blender. Don't forget to reset the material before baking. A preview feature is planned for a future release.

How do I know when there is an updated version of LightBake?
There is a link in the release notes to the latest version of the plugin. You can also check the plugin page on the Blender Market.

### There are artifacts on my lightmaps?

Check if you have proper UV unwrap settings and good enough rendering quality (samples, time limit). Blender denoise has limitations with lightmaps.

### Baking time is slow

In advanced settings (You can enable it in Render > Light bake sub property panels), a time limit allows control over time per element. These settings are available in the render panel for both lightmaps and probes. You can also use quick render to test settings.

### Tips?

Setting up lights and initially working with reflection probes seems to be the fastest way to check lighting conditions. Then, proceed with the irradiance grid and finally lightmaps. As lightmaps take time to render, using quick render and low resolution can help test consistency.

Using lightmaps to render only direct and indirect diffuse allows the use of low-resolution lightmaps and achieves good results. However, it requires continued use of the original albedo.

The OpenEXR format is the best for HDR maps, but it is heavy. PNG can be a good alternative but requires adjusting exposure options for both lightmaps and probes.

For an optimal lightmap pack, grouping objects with the same size helps maintain consistent lightmap quality and provides more control. Having one object per group becomes the best way to adjust quality. More objects in large maps will result in an optimal pack layout.

Using UV unwrap in lightmap pack is the basic option if the object geometry has been properly set for unwrapping. It helps avoid most denoiser artifacts. Otherwise, Smart UV project can be used for objects with large surfaces, especially walls and floors. In the last resort, lightmap pack can be used, but it has a margin, wasting a lot of map space and creating artifacts and small faces.