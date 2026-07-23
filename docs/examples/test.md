---
title: "Example Test - Test"
tier: Examples
audience: [author]
sources:
  - examples/03-signing/signed-stool-and-bucket.json
  - examples/03-signing/README.md
verified: c9029f4
nav:
  prev: examples/02-stool-and-bucket.md
  next: examples/building-signmsf.md
---

# Example Stool

This example will guide you through making a very simple spatial fabric. By the end of it you will have put one 3D object on the screen, and you will understand the handful of parts that every other example is built from.

## Prerequisites

1. Artemis Browser
1. Web Server (local or Hosted)

## Quickstart

If you want to see what the final output looks like, just follow these quick instructions.

1. Create a directory/folder (i.e. my-fabric) for the spatial fabric 
1. Download the [initial project skeleton](https://cdn.rp1.com/sneeze/examples/stool.zip) and unzip its contents in the directory you just created. Make sure that the resulting structure looks like this:
```text
my-fabric/
├── stool.json
├── wasm/
│   └── map.wasm
└── assets/
    └── Stool.glb
```
1. Copy the `my-fabric` folder onto your webserver
1. Launch Artemis
1. In the address bar of Artemis, use `https://<your-webserver>/my-fabric/stool.json`
![ss_stool.jpg](/assets/ss_stool.jpg)

## Details

Here is `stool.json` file:

```json
{
   "Container": "example-stool",
   "Services": [
      
   ],
   "Modules": [
      {
         "sUrl": "wasm/map.wasm"
      }
   ],
   "Data": {
      "Scene": {
         "Head": {
            "Self":"P-?"
         },
         "Name": "Stool",
         "Resource": {
            "sReference": "assets/Stool.glb"
         }
      }
   }
}
```

## What does each data block do?

**`Container`** defines a container identifier that is used to group together fabrics that you publish into executable units. Spatial fabrics that you publish with the same container identifier will run in the same container, sharing network connections, storage space, cached files, and console output. If you create separate fabrics that you want to run in separate containers, simply give each one their own identifier. Know that you can only share containers among fabrics that you or your organization publishes. You'll learn more about that a little later.

**`Services`** describes the connection settings for outside services that a running module connects to, such as a map or a live data source. This example does not utilize services, so the list is empty. Services are covered in a later example.

**`Modules`** lists the programs the fabric runs. This example lists one module, `map.wasm`, which is a general-purpose program that we'll examine in a later example. The job of `map.wasm` is to read a tree of objects out of the `data` section and turn each object into a node in the scene, which is exactly why this fabric can show a stool without you writing any code of your own. If you provide a scene but list no module to interpret it, nothing would be added to the scene.

**`Data`** is a general block of information the fabric carries for its modules to read. In this example, `map.wasm` reads the **`Data.Scene`** for the tree of objects that makes up the scene: 

```json
{
   // ... other values
      "Scene": {
         "Head": {
            "Self":"P-?"
         },
         "Name": "Stool",
         "Resource": {
            "sReference": "assets/Stool.glb"
         }
      }
   }
}
```

- **`Head.Self`** is the object's identifier. The nomenclature is the following:
    - Object Class Type, which is a single character:
        - **`R`** Root Object, meaning ...
        - **`C`** Celestial Object, meaning ...
        - **`T`** Terrestial Object, meaning ...
        - **`P`** Physical Object, meaning an ordinary solid thing.
        - **`L`** Light Object, meaning ...
    - Hyphen, which is just a separator
    - Index, which is an identifier which can be:
        - **`?`** The engine will assign the next free index in the container automatically. In this example, we use `?`.
        - **`Number`** This assigns a specific number to identify this object. Note, this number **must be unique** within a container, otherwise you will have a collision. Typically it is safer to use `?` to avoid collision.
- **`Name`** is a readable label for the object. Here it is `"Stool"`. It is for your benefit and does not affect what is drawn.
- **`Resource.sReference`** is the address of the 3D model to draw for this object. When the engine builds this node, it downloads this `.glb` file and draws it. The paths are relative to location of the spatial fabric. In this example, we will load `Stool.glb` in a subfolder named `assets`.

## Where the light comes from

This fabric does not describe any lights -- we'll introduce lighting in the next example. A scene with no light in it would be pure black and invisible, so when a fabric provides no lighting of its own, the engine falls back to a plain ambient light: a soft, even fill that arrives from every direction at once. That fallback is the only reason you can see the stool here at all. Because ambient light has no direction, it reveals the model's colour and form flatly -- there are no bright highlights or cast shadows, since those only appear when light comes from a definite direction. The next examples add lights on purpose, so that you -- not the fallback -- decide how the scene looks and where the shadows fall.
