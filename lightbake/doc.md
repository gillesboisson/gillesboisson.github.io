---
layout: lightbake
permalink: /light-bake/doc
meta_description: >
  LightBake is a light baking solution for Blender. It allows you to bake light for real-time rendering engines and comprises two key tools: a lightmap baking system and a probes baking system.

meta_keywords: Blender, Plugin, 3D, GI, Baking, FAQ
section: lightbake-doc

---

![logo](/assets/lightbake/images/logo.png)

Light Bake is a Blender plugin that bakes light for real-time rendering. It bakes light in two forms: probes and lightmaps. The baked content is exported in a JSON file with bake data and textures (PNG or OpenEXR).

### Example

![Intro](/assets/lightbake/images/intro.png)

![Render](/assets/lightbake/images/victorian-iso.png)

## Light Maps

Setting up light maps can be long and tedious. Light Bake's lightmaps allow you to create light presets, link them to objects, and bake them. The plugin supports diffuse, combined, and AO Blender baking types, automating UV unwrapping, texture packing, and baking.

![Lightmap](/assets/lightbake/images/lightmap-uv.png)
<br/>*Lightmap group example*

![Lightmaps](/assets/lightbake/images/three-lightmap-render.png)
<br/>*Lightmap-only integration*

Group packing allows you to pack multiple objects into one texture. It is useful for small objects, and an object per pack allows you to keep final map quality no matter the number of objects.

Collection visibility options define which objects are visible during baking. It helps optimize rendering time and separates static and active objects in a scene. 

The plugin fixes the Blender denoiser issue with baked maps, as it uses the Blender composer manually after baking to denoise the image. This facilitates having quick good rendering.

UV unwrapping options help define precisely how UV unwrapping is done. It can be automated or done manually (See Advanced mode).
## Baking Process

Light Bake uses Blender's internal baking and renders internally, utilizing the Cycles rendering engine to bake probes and denoise lightmaps.

Bake maps and probes are rendered in a `__render_cache` subdirectory. It is used to store intermediate rendered files and avoids re-rendering everything on each bake. The cache can be cleared manually for each element or for all from Properties / View 3D panels.

The "Bake only none cache" option (View 3D > Bake GI) will limit the bake to non-rendered elements when "Bake probes/lightmaps" is used.

## Probes

Probes are spherical volumes that capture light in a scene. They are used to compute indirect light and reflection maps in a scene. In contrast to lightmaps, they are used in active objects by providing surrounding light information.

The plugin is based on Blender Eevee probes implementation, using most of Eevee probe data options for rendering and extra options for baking (See Advanced mode).

As probe data is exported as a cubemap pack (See data), extra options are available to define cubemap pack size and final texture size.

### Irradiance Grid

The irradiance grid is a grid of probes that capture light in a scene. It is used to compute indirect light. Several options are available to define grid resolution and influence.

![Probes](/assets/lightbake/images/irradiance-probes.png)

![Probes](/assets/lightbake/images/threejs-irradiance-probes.png)

### Reflection Probes

The reflection probe volume is a spherical volume that captures reflected light in a scene (Radiance). It is used to compute reflection maps in a scene. For real-time optimization purposes, reflection is captured at several roughness levels and exported as a cubemap face pack (See data) with roughness levels.

![Reflection probe](/assets/lightbake/images/reflection-probe.png)

![Reflection probe](/assets/lightbake/images/threejs-probe-reflection.png)

### Environment/Global

The environment is a combination of irradiance and reflection probes. It is used as a fallback when active objects are not in the range of probes. Apart from that, it has the same options as irradiance and reflection probes. As it is used as a fallback, it is recommended to use a specific collection for visibility that represents the scene environment.

## Panels

Light Bake has panels in Properties and 3D view. By default, only main properties are available, and advanced options (Properties > Render) allow for advanced properties.

### Export Setup (Properties > Output)

Setup where and how bake information is exported. This has to be defined first; otherwise, other tools are not available. Export is done automatically on each bake and can be done manually. It is presented as a file browser with extra options (Includes GLTF options when GLTF export is enabled).

![export](/assets/lightbake/images/export-file-browser.png)

### Render (Properties > Render)

- Bake operators
- Display global and default baking options

#### Maps

- Edit maps render settings 
- Edit presets
- Bake preset / all maps

By default, maps use common rendering settings, but can be edited individually (**Use default settings**).

An extra quick render options limit render to 1 sec per maps for testing purposes.

An advanced mode is available with more options: Rendering, Wrapping.

![render maps panels](/assets/lightbake/images/render-maps-panels.png)

*Advanced*

![render maps panels](/assets/lightbake/images/render-maps-panels-advanced.png)

#### Probes

- Edit probes default rendering settings 
- Bake all probes with extra bake settings.

![probes render panel](/assets/lightbake/images/probes-render-panel.png)

*Advanced*

![probes render panel](/assets/lightbake/images/probes-render-panel-advanced.png)

### Object Lightmap (Properties > Object)

- Link object to existing lightmap preset or create a new one.
- Setup preset options (Same as render > preset edit) and bake.

![object lightmap panel](/assets/lightbake/images/object-lightmap-panel.png)

*Advanced*

![object lightmap panel](/assets/lightbake/images/object-lightmap-panel-advanced.png)

### Probe (Properties > Probe Data)

As the plugin uses Eevee probes, most settings are based on Eevee probes settings. 

Extra options are used only to use custom baking settings, as by default, probes use default settings (*Rendering panel > Probes volume*).

#### Irradiance Grid

![irradiance grid panel](/assets/lightbake/images/irradiance-probe-data-panel.png)

*Advanced*

![irradiance grid panel](/assets/lightbake/images/irradiance-probe-data-panel-advanced.png)

#### Reflection Probe

Reflection Eevee probes are used for both radiance and global probes.

![reflection probe panel](/assets/lightbake/images/reflection-probe-data-panel.png)

*Advanced*

![reflection probe panel](/assets/lightbake/images/reflection-probe-data-panel-advanced.png)

#### Global Probe

Reflection probe becomes a global probe with "Use as global probe" options. The panel becomes the same as Render > Probe Volumes > Global Probe.

![global probe panel](/assets/lightbake/images/probe-global-data-panel.png)

*Advanced*

![global probe panel](/assets/lightbake/images/probe-global-data-panel-advanced.png)

### Collection Visibility (Properties > Collection)

As a collection can be defined as a visibility filter in probes and lightmaps, extra layer mask settings can be defined in the collection properties panel to export them.

![Collection panel](/assets/lightbake/images/collection-panel.png)

### 3D View

Global view of baked elements in 3D view with shortcuts to elements baking operations.

![3D view panel](/assets/lightbake/images/3d-view-panel.png)

## Data

Bake data is exported in a JSON file with textures. Two formats are supported for data: JSON with bake information only and GLTF with bake information as scene custom props and scene.

```json

{
    // Probes list
    "probes": [

        // Global probe example
        {
           
            "name": "env",
            // global | irradiance | reflection
            "probe_type": "global",

            // transform data
            "transform": {
                "position": [
                    1.370906801412275e-07,
                    0.5399999022483826,
                    1.4375
                ],
                "scale": [
                    5.599999904632568,
                    11.199999809265137,
                    2.799999952316284
                ],

                // euler rotation
                "rotation": [
                    0,
                    0,
                    0
                ]
            },

            
            "render": {
                "clip_start": 0.800000011920929,
                "clip_end": 40.0,
                "map_size": 256,
                "cycle_samples_max": 32,
                "cycle_samples_min": 1,
                "cycle_time_limit": 0,
                "cycle_use_denoising": true
            },

            // files location
            "irradiance_file": "textures/env_irradiance_packed.exr",
            "reflection_file": "textures/env_radiance_packed.exr",
            
            // baking options
            "baking": {
                "irradiance": {
                    "cubemap_face_size": 32,
                    "max_texture_size": 3072
                },
                "reflection": {
                    "cubemap_face_size": 128,
                    "start_roughness": 0.10000000149011612,
                    "end_roughness": 0.8999999761581421,
                    "nb_levels": 4
                }
            },

            // collection visibility
            "visibility": {
                "collection": "env",
                "invert": false
            }
        },

        // Reflection example
        {
            "name": "ground_reflection",
            "probe_type": "reflection",
            "transform": {
                "position": [
                    1.370906801412275e-07,
                    0.5399999022483826,
                    1.4375
                ],
                "scale": [
                    5.599999904632568,
                    2.799999952316284,
                    11.199999809265137
                ],
                "rotation": [
                    0,
                    0,
                    0
                ]
            },
            "render": {
                "clip_start": 0.800000011920929,
                "clip_end": 1000.0,
                "map_size": 256,
                "cycle_samples_max": 32,
                "cycle_samples_min": 1,
                "cycle_time_limit": 0,
                "cycle_use_denoising": true
            },
            "data": {
                "falloff": 0.20000000298023224,
                "intensity": 1.0,
                "influence_type": "BOX",
                "influence_distance": 2.4000000953674316
            },
            "file": "textures/ground_reflection_packed.exr",
            "baking": {
                "cubemap_face_size": 256,
                "start_roughness": 0.10000000149011612,
                "end_roughness": 0.8999999761581421,
                "nb_levels": 4
            },
            "visibility": {
                "collection": "static",
                "invert": false
            }
        },

        // Irradiance example
        {
            "name": "top_irradiance",
            "probe_type": "irradiance",
            "transform": {
                "position": [
                    1.370906801412275e-07,
                    9.5,
                    1.4375
                ],
                "scale": [
                    5.039999961853027,
                    3.3600001335144043,
                    16.282176971435547
                ],
                "rotation": [
                    0.0,
                    0.0,
                    0.0
                ]
            },
            "render": {
                "clip_start": 0.009999999776482582,
                "clip_end": 1000.0,
                "map_size": 64,
                "cycle_samples_max": 16,
                "cycle_samples_min": 1,
                "cycle_time_limit": 0,
                "cycle_use_denoising": true
            },
            "file": "textures/top_irradiance_packed.exr",
            "data": {
                "resolution": [
                    1,
                    1,
                    3
                ],
                "intensity": 1.0,
                "influence_distance": 0.30000001192092896,
                "falloff": 1.0
            },
            "baking": {
                "cubemap_face_size": 32,
                "max_texture_size": 1024
            },
            "visibility": {
                "collection": "static",
                "invert": false
            }
        }
    ],

    // Bake map data
    "baked_maps": [

        // Ambient occlusion example
        {
            "map_size":512,
            "type":"AO",
            "name":"ao_test_AO",
            "grouped_bake":true,

            // based on blender bake types / passes
            "passes":[
                "AO"
            ],
            "format":"HDR",

            // UV layer index, in this case a second uv layer is used
            "uv_index":1,
            "visibility":{
                "collection":"static",
                "invert":false
            },
            "maps":[
                {
                    "filename":"textures/ao_test_AO.exr",
                    "objects":[
                        "ao_cube",
                        "ao_cube.001"
                    ]
                }
            ]
        },

        // packed diffuse example
        {
            "map_size":1024,
            "type":"DIFFUSE",
            "name":"floor_ceiling_DIFFUSE",
            
            "passes":[
                "DIRECT",
                "INDIRECT"
            ],
            "format":"SDR",
            "uv_index":1,
            "visibility":{
                "collection":"static",
                "invert":false
            },

            // in this example, two objects are packed in one texture
            "grouped_bake":true,
            "maps":[
                {
                    "filename":"textures/floor_ceiling_DIFFUSE.png",
                    "objects":[
                        "ceiling",
                        "floor"
                    ]
                }
            ]
        },

        // Packed group with object per map example
        {
            "map_size":512,
            "type":"DIFFUSE",
            "name":"columns_DIFFUSE",
            "passes":[
                "DIRECT",
                "INDIRECT"
            ],
            "format":"HDR",
            "uv_index":1,
            "visibility":{
                "collection":"static",
                "invert":false
            },

            // as the map is grouped and is limited to 3 objects, for 6 columns, two maps are created
            "grouped_bake":true,
            "maps":[
                {
                    "filename":"textures/columns_0_DIFFUSE.exr",
                    "objects":[
                        "Cube",
                        "Cube.001",
                        "Cube.002"
                    ]
                },
                {
                    "filename":"textures/columns_1_DIFFUSE.exr",
                    "objects":[
                        "Cube.003",
                        "Cube.004",
                        "Cube.005"
                    ]
                }
            ]
        },
    ],

    // Visibility collections indicate for each collection used during baked which objects are visible, this can be used in realtime engine to separate static and active objects or split rendering in several passes for optimization purpose
    // !! collections used in blender but not used as visibisity constrain by probes or maps are not listed
    "collections": [
        {
            "name": "static",
            "objects": [
                "Cube",
                "Cube.001",
                "Cube.002",
                "Cube.003",
                "Cube.004",
                "Cube.005",
                "ceiling",
                "floor",
                "wall_top",
                "walls",
                "Point",
                "Point.001",
                "Point.002",
                "Point.003",
                "Point.004",
                "Point.005",
                "Point.006",
                "Point.007",
                "Night light"
            ],
            "baked_by_probes": [
                "default",
                "IrradianceVolume",
                "reflection left",
                "reflection right"
              ],
              "baked_by_light_maps": [
                "furniture_DIFFUSE",
                "deco_DIFFUSE",
                "floor_DIFFUSE",
                "sofa_DIFFUSE",
                "Rug_Large_DIFFUSE",
                "Rug_Small_DIFFUSE",
                "walls_DIFFUSE"
              ],
        },
        {
            // if layer mask enabled a binary mask is set
            "layer_bit_mask": 1,
            "name": "use lightmaps",
            "baked_by_probes": [],
            "baked_by_light_maps": []
        },
        {
            "layer_bit_mask": 2,
            "name": "use only probes",
            "baked_by_probes": [],
            "baked_by_light_maps": []
        },
        
    ]
}

```


### GLTF Data format

GLTF is based on the existing Blender GLTF with extra options. Bake data is added to the scene's extra properties: `scene.extra.bake_gi_maps`. The GLTF file will be exported automatically on each bake and can be done manually (View 3D > Bake GI).

```json
{
	// ...
	"scene":0,
	"scenes":[
		{
			"extras":{
				"bake_gi_export_json":{
                    // Same as JSON data
                }
            }
        }
    ],
    // ...
    "nodes":[
        {
			"extras":{
                // extra nodes data
				"bake_gi_export_json":{

                    // collection visibility (based on scene json data)
					"collections":[
						"static"
					],

                    // map links
					"map":{
                        // preset index in scene.extra.bake_gi_export_json.baked_maps
						"pack":0,

                        // map index in scene.extra.bake_gi_export_json.baked_maps[pack].maps
						"map":1
					}
				}
			},
			"mesh":3,
			"name":"Cube.003",
			"scale":[
				5.599999904632568,
				5.599999904632568,
				5.599999904632568
			],
			"translation":[
				5.039999008178711,
				-5.059999942779541,
				1.4375
			]
		},
    ]
}
```
## Maps

Maps can be exported in two formats: PNG and OpenEXR. PNG is used for SDR maps (8 / 16 ushort depth), and OpenEXR is used for HDR maps (16 | 32 float color depth). 

### Lightmaps

Lightmaps are packed based on the lightmap preset setup and object UV. Each preset has a map size and pack configuration, with three pack configurations available:

- **None group**: each object has its own map: `[texture_directory]/[object_name].[png|exr]` (e.g., `./textures/my_object.png`)
- **Grouped**: all objects are packed into one map: `[texture_directory]/[preset_name].[png|exr]` (e.g., `./textures/my_preset.png`)
- **Grouped with objects per map limit**: all objects are packed into several grouped maps: `[texture_directory]/[preset_name]_[group_index].[png|exr]` (e.g., `./textures/my_preset_[0].png`)

(See JSON data format for more details).

### Probes

Probes are exported as cubemap face packs. Faces are exported in a single file per probe volumes. The layout is specified for each probe type.

#### Reflection probes

For one reflection probe, there is one file with several cubemaps (one per roughness level). They have a fixed layout:

![Reflection probe](/assets/lightbake/images/ground_reflection_packed-info.png)

#### Irradiance probes

For one irradiance probes volume, there is one file with several cubemaps. They have a fixed layout based on pack map width (defined in probe settings). Six faces are in the same rows, and the final map width is a multiple of 6 x face size.

![Irradiance probe](/assets/lightbake/images/ground_irradiance_packed-info.png)

## Lighting Baking Details

### Irradiance Probe

Irradiance computing is based on the Diffuse Irradiance Volume OpenGL implementation from [Learn OpenGL](https://learnopengl.com/PBR/IBL/Diffuse-irradiance) implementation.

### Radiance Probe

Radiance computing is based on the IBL Volume implementation from [Learn OpenGL](https://learnopengl.com/PBR/IBL/Specular-IBL) implementation.

## Engine Integration

An existing integration with Three JS exists as an extension; it supports all features and is used for preview. It is available on [GitHub](https://github.com/gillesboisson/threejs-probes-test) with a [preview demo here](https://three-probes.dotify.eu/).
