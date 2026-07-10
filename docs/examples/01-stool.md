---
title: "Example 01 - A Single Stool"
tier: Examples
audience: [author]
sources:
  - examples/01-stool/stool.json
  - examples/01-stool/README.md
verified: b3d15ea
nav:
  prev: examples/index.md
  next: examples/02-stool-and-bucket.md
---

# Example 01 - A Single Stool

This example will guide you through making a very simple spatial fabric by creating authoring a metaverse spatial fabric (MSF) file. By the end of it you will have put one 3D object on the screen, and you will understand the handful of parts that every other example is built from. This example assumes you have a metaverse browser, such as [Artemis](https://rp1.com/Artemis).

## What is a Metaverse Spatial Fabric?

An MSF file is a 3D space described as data. Instead of writing a program that draws a scene, you write a file that *describes* what is in the scene, and the engine reads that file and draws it for you. This example's fabric is `stool.json`, and it describes a scene containing exactly one thing: a stool.

An MSF file is to a metaverse browser Engine what an HTML file is to a Web Browser Engine. It describes a 3D space, that the engine will render and allow the user to move through.

## What This Example Teaches

- What a fabric file looks like and what each part of it is for.
- How a scene is built from nodes, where a node is a single thing in the space.
- How to attach a 3D model (a `.glb` file) to a node so it appears on screen.
- Where the light comes from when you have not added any lights of your own.

## The files

These files can be found in the `Sneeze Example` Repository.

| File | What it is |
|---|---|
| `{SNEEZE_EXAMPLE_REPO}/examples/01-stool/stool.json` | The fabric. This one file is the entire scene. |
| `{SNEEZE_EXAMPLE_REPO}/examples/wasm/map.wasm` | The stock module that reads map nodes from `data.scene` and turns it into a scene. It lives in a shared `wasm` folder because every map-managed example uses the same one. |
| `{SNEEZE_EXAMPLE_REPO}/assets/Stool.glb` | The 3D model of the stool. It lives in a shared `assets` folder because later examples reuse it. |

## The spatial fabric, line by line

**Stool.json**

```json
{
   "container": "example-stool",
   "services": [],
   "modules":
   [
      {
         "url": "wasm/map.wasm"
      }
   ],
   "data":
   {
      "scene":
      { 
         "Head": { 
            "Self": "P-?" 
         }, 
         "Name": "Stool", 
         "Resource": {
            "sReference": "assets/Stool.glb" 
         }
      }
   }
}
```

**`container`** defines a container identifier that is used to group together fabrics that you publish into executable units. Spatial fabrics that you publish with the same container identifier will run in the same container, sharing network connections, storage space, cached files, and console output. If you create separate fabrics that you want to run in separate containers, simply give each one their own identifier. Know that you can only share containers among fabrics that you or your organization publishes. You'll learn more about that a little later.

**`services`** is reserved for future use. It will describe the connection settings for outside services that a running module connects to, such as a map or a live data source. 

**`modules`** lists the programs the fabric runs. This example lists one module, `map.wasm`, which is a general-purpose program that we'll examine in a later example. The job of `map.wasm` is to read a tree of objects out of the `data` section and turn each object into a node in the scene, which is exactly why this fabric can show a stool without you writing any code of your own. If you provide a scene but list no module to interpret it, nothing would be added to the scene. Building your own module is covered in a later example.

**`data`** is a general block of information the fabric carries for its modules to read; you can put anything you want in it. The `map.wasm` program we're running looks in one specific place inside it -- **`data.scene`** -- for the tree of objects that makes up the scene. In this particular example, `data.scene` is just a single object. Its three parts are:

- **`Head.Self`** is the object's identifier, written as a class letter, a hyphen, and an index. The letter is the kind of object and the index is which one it is within its container. `P` indicates the node is a physical object, meaning an ordinary solid thing. Here, instead of a fixed number, the index is a `?`, as in `"P-?"`. The `?` tells the engine to assign the next free index in the container automatically, rather than you hard-coding one. This matters because more than one published fabric can be loaded into the same container, and if each hard-coded its own `P-1` the identifiers would collide. Letting the engine hand out the index keeps every object unique no matter how many fabrics share the container. You can still write a fixed index like `"P-1"` when you deliberately want to name a specific object, but `"P-?"` is the safe default.

- **`Name`** is a readable label for the object. Here it is `"Stool"`. It is for your benefit and does not affect what is drawn.

- **`Resource.sReference`** is the address of the 3D model to draw for this object. When the engine builds this node, it downloads this `.glb` file and draws it.

That is the entire scene: a fabric that runs the map module, which reads one physical object, which draws one model.

> These instructions created an unsigned JSON file, which is convenient while you are learning, but it is not how a MSF file is meant to be published and it may not always work. Metaverse browsers will expect an MSF file to be *signed*. A later example will show you how to sign your JSON file.
{.is-warning}

## Deploying it so the browser can load it

To deploy this example:

1. Upload `stool.json` to your own web server so it has a public address.
2. Upload `Stool.glb` to the location specified in the MSF file. If you copied `data` as in the example above, you would upload it to a folder named `assets`.
3. Upload `map.wasm` to the location specified in the MSF file. If you copied `modules` as in the example above, you would upload it to a folder named `wasm`.

## Where the light comes from

This fabric does not describe any lights -- we'll introduce lighting in the next example. A scene with no light in it would be pure black and invisible, so when a fabric provides no lighting of its own, the engine falls back to a plain ambient light: a soft, even fill that arrives from every direction at once. That fallback is the only reason you can see the stool here at all. Because ambient light has no direction, it reveals the model's colour and form flatly -- there are no bright highlights or cast shadows, since those only appear when light comes from a definite direction. The next examples add lights on purpose, so that you -- not the fallback -- decide how the scene looks and where the shadows fall.

## Complete

Now open your metaverse browser, and load your MSF file. You should see a stool.

## What is next

Example 02 keeps the same single model but places it deliberately using a position, rotation, and size. That is the groundwork for scenes that hold more than one object, since once you can place one object exactly where you want it, you can place many.

---

[Examples](index.md) | [Example 02](02-stool-and-bucket.md) | [Home](../Home.md)
