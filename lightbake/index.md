---
layout: lightbake
permalink: /light-bake
meta_description: >
  LightBake is a light baking solution for Blender. It allows you to bake light for real-time rendering engines and comprises two key tools: a lightmap baking system and a probes baking system.

meta_keywords: Blender, Plugin, 3D, GI, Baking
section: lightbake
---

*Light baking simplified.*

![logo](/assets/lightbake/images/logo.png)

![logo](/assets/lightbake/images/victorian-iso.png)

## Introduction

LightBake is a light baking solution for Blender. It allows you to bake light for real-time rendering engines and comprises two key tools: a lightmap baking system and a probes baking system.

The lightmap utilizes existing Blender baking but handles most of the setup and export processes for you. This feature is particularly helpful with complex scenes and ensures a consistent lightmap quality. It solves the blender denoise legacy issue with diffuse lightmaps.

The Probes tool is based on Eevee's probe system for the authoring part and enables the export of probes as external images in an optimal format for use in real-time rendering engines.

![Schema](/assets/lightbake/images/schema.png)

## Features

Lightbake has modular approach as it can used for any kind of light baking. It can be used to bake Global illumination only using a mix of indirect lightmaps and probes or direct lighting and color lightmaps only for high quality static scenes. Most default settings can be used for most scenes, but it also has advanced settings for more control. 

### Easy lightmap baking

With its lightmap preset system, it allows you to set up lightmaps, link objects to them, and bake them in packs.

![lightmap baking](/assets/lightbake/images/victorian-iso-lightmap.png)

- Lightmaps can be packed individually for each object with an additional option for an extra object per map.
- Supported baking types include Diffuse, Combined, and AO, with options for sub-passes.
- Lightmap resolution and bake quality can be easily adjusted through rendering and baking options.
- The plugin addresses the legacy issue in Blender related to lightmap denoising by introducing an extra compositing step.

![object lightmap panel](/assets/lightbake/images/render-maps-panels.png)
<!-- ![object lightmap panel](/assets/lightbake/images/map-object-panel.png) -->

### Probes volume baking

The main purpose of Probes is to provide global illumination and reflections for scenes. It is used to apply light to active objects in the scene or objects that do not have lightmaps.

Building upon the existing Eevee probes system, LightBake introduces additional options and operations for probe baking. It enables the baking of probes into a texture pack. Extra data related to influence (based on Eevee integration) is exported to provide more control to the rendering engine.

![probes baking](/assets/lightbake/images/victorian-iso-probes.png)

**Radiance map pack**

Baking reflection (radiance) volumes involves precomputing roughness in a multi-level cubemap pack.

![Radiance map pack](/assets/lightbake/images/packed-radiance.png)

**Irradiance map pack**

Baking the diffuse (irradiance) volume grid involves precomputing irradiance cubemap pack.

![Irradiance map pack](/assets/lightbake/images/packed-irradiance-volume.png)

**Global fallback probe**

A combination of irradiance and radiance probes is used to create a fallback light environment.

### Authoring, Baking, and Caching

Bake actions are accessible from the properties panel and the 3D View. Rendered content is saved in a cache folder, enabling a gradual element-wise baking process. An additional quick render option aids in content baking for testing.

![Quick render panel](/assets/lightbake/images/view3D-panels.png)

### Visibility Collection

Baked probes and lightmaps can be organized within a collection. This optimization helps reduce rendering time and allows content grouping by baking type. Additional layer mask options are available in the collection properties panel to guide external rendering engines on how to perform render passes and organize lights.

![Collection panel](/assets/lightbake/images/collection-panel.png)



### Export Data and Texture Format

Data can be exported in a separate JSON file or integrated into a glTF file. Probes and lightmaps can be exported in two formats: OpenEXR for high dynamic range and PNG for low dynamic range. Configuration is similar to GLTF export; data is exported automatically after each bake.

[More details on data documentation](./doc.md)

![Export setting panel](/assets/lightbake/images/export-file-browser.png)

## Integration with Real-time Engine

No specific integration has been done for any known engine. Exported content is based on modern baking standards and can be used in any engine that supports lightmaps or probes. A Three.js integration is available and has been used for preview. It is available on [GitHub](https://github.com/gillesboisson/threejs-probes-test). The scene used for this preview is available here: [Victorian room](https://three-probes.dotify.eu/) 

## Limitations

As LightBake's main goal is to bake light for real-time rendering engines, there is no way to preview rendering lightmaps or probes in Blender (for now). It is also not a substitute for any other baking plugins like Simple Bake, as it doesn't support other bake modes than diffuse, combined, and AO. It can work alongside other bake plugins. 

### Compatibility

LightBake is compatible with the last 2 Blender LTS Versions (3.3 and 3.6) and is also compatible with Blender 4+.

### License 

The Light Bake plugin is released on Blender Market under the GPL License.

The Victorian room scene is a modified version of Matthew Collings' scene released on [Sketchfab](https://sketchfab.com/3d-models/victorian-room-d6b81ab3924443f99ae4542c6be4e1a6) under the Sketchfab standard license and is not included with this plugin.