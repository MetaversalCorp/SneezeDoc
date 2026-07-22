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
1. In the address bar of Artemis, use `https:<your-webserver>/my-fabric/stool.json`
1. You should now see your stool fabric.

## Details

Here is the whole file:

```json
{
   "Container":"example-stool",
   "Services":[
      
   ],
   "Modules":[
      {
         "sUrl":"wasm/map.wasm"
      }
   ],
   "Data":{
      "Scene":{
         "Head":{
            "Self":"P-?"
         },
         "Name":"Stool",
         "Resource":{
            "sReference":"assets/Stool.glb"
         }
      }
   }
}
```
<details close>
   <summary>What does each data block do?</summary>

**`Container`** defines a container identifier that is used to group together fabrics that you publish into executable units. Spatial fabrics that you publish with the same container identifier will run in the same container, sharing network connections, storage space, cached files, and console output. If you create separate fabrics that you want to run in separate containers, simply give each one their own identifier. Know that you can only share containers among fabrics that you or your organization publishes. You'll learn more about that a little later.

**`Services`** describes the connection settings for outside services that a running module connects to, such as a map or a live data source. This example does not utilize services, so the list is empty. Services are covered in a later example.

**`Modules`** lists the programs the fabric runs. This example lists one module, `map.wasm`, which is a general-purpose program that we'll examine in a later example. The job of `map.wasm` is to read a tree of objects out of the `data` section and turn each object into a node in the scene, which is exactly why this fabric can show a stool without you writing any code of your own. If you provide a scene but list no module to interpret it, nothing would be added to the scene.

**`Data`** is a general block of information the fabric carries for its modules to read; you can put anything you want in it. The `map.wasm` program we're running looks in one specific place inside it -- **`Data.Scene`** -- for the tree of objects that makes up the scene. In this particular example, `data.scene` is just a single object. Its three parts are:

- **`Head.Self`** is the object's identifier, written as a class letter, a hyphen, and an index. The letter is the kind of object and the index is which one it is within its container. `P` indicates the node is a physical object, meaning an ordinary solid thing. Here, instead of a fixed number, the index is a `?`, as in `"P-?"`. The `?` tells the engine to assign the next free index in the container automatically, rather than you hard-coding one. This matters because more than one published fabric can be loaded into the same container, and if each hard-coded its own `P-1` the identifiers would collide. Letting the engine hand out the index keeps every object unique no matter how many fabrics share the container. You can still write a fixed index like `"P-1"` when you deliberately want to name a specific object, but `"P-?"` is the safe default.
- **`Name`** is a readable label for the object. Here it is `"Stool"`. It is for your benefit and does not affect what is drawn.
- **`Resource.sReference`** is the address of the 3D model to draw for this object. When the engine builds this node, it downloads this `.glb` file and draws it.

That is the entire scene: a fabric that runs the map module, which reads one physical object, which draws one model.
</details>

## Deploying it so the browser can load it

The finished metaverse browser will not (yet) load a fabric from your local disk; it loads it from the network, the same way a web browser loads a page from a web address. Each thing the fabric names - the module in `modules` and the model in `data` - is given as an address, and that address can be written two ways. A full address, one that includes a `scheme://` such as `https://...`, is used exactly as written. Anything shorter is a relative address, resolved against the fabric's own location just as a web browser resolves a relative link on a page: a plain name like `assets/Stool.glb` is looked up in the folder the fabric lives in, a leading `/` starts from the host root, and `..` steps up a folder. That is why this fabric can simply say `wasm/map.wasm` and `assets/Stool.glb` - both sit alongside `stool.json`, so they resolve to `.../sneeze/examples/wasm/map.wasm` and `.../sneeze/examples/assets/Stool.glb`. Either way, every file the fabric references has to end up at an address the browser can reach over the internet.

## Where the light comes from

This fabric does not describe any lights -- we'll introduce lighting in the next example. A scene with no light in it would be pure black and invisible, so when a fabric provides no lighting of its own, the engine falls back to a plain ambient light: a soft, even fill that arrives from every direction at once. That fallback is the only reason you can see the stool here at all. Because ambient light has no direction, it reveals the model's colour and form flatly -- there are no bright highlights or cast shadows, since those only appear when light comes from a definite direction. The next examples add lights on purpose, so that you -- not the fallback -- decide how the scene looks and where the shadows fall.
