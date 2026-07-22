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

# Example Test - Publishing a Test Signed Spatial Fabric

This test example builds on [Example 02](../02-stool-and-bucket/README.md), but it does not change the scene at all. The stool, the bucket, and the lighting - the `Primary` block's ambient and directional light plus the two spot lights - are exactly the same. What changes is how the fabric is prepared for the world to load: we pin its module to an exact version with a hash, and then we sign the whole fabric into a Metaverse Spatial Fabric (`.msf`) file. This is the preferred way to publish a spatial fabric because it provides the security necessary to identify you as the publisher of the fabric.

## ⚠️ The following warning paragraph is different from the ones in Examples 01 & 02



## ⚠️ Fabric security measures are not fully utilized... yet

At present, the browser computes and reports a fabric's trust level, but it does not yet refuse a fabric for being unsigned or untrusted - it loads them all today. So signing right now is about attaching a real, verifiable identity to your work and being ready for when enforcement arrives; it is not yet a gate that keeps unsigned fabrics out. Sign your published fabrics anyway - the identity inside them is genuine and future-proof.

## Why sign a fabric?

A plain JSON fabric works while you are learning, but it cannot prove who wrote it, and it cannot prove that what the browser downloaded is what the author actually published. Anyone could alter the file in transit, or serve a different file entirely, and the browser would have no way to tell. Signing fixes both. A signed fabric carries the author's certificate inside it, so the browser learns who published it, and it carries a cryptographic signature over the exact bytes of the payload, so the browser can tell if a single character was changed. The signed file is the real, publishable form of a fabric.

## What this example teaches

- How to pin a module to an exact version with a hash, so the browser refuses a module that does not match.
- Where the ready-made signing credentials live for local testing, and how you get your own for real publishing.
- How to sign a fabric into a `.msf` file using the metaverse browser you already have, and how to verify the result before you publish it.



## What is new since Example 02

The scene is identical to Example 02, so nothing in the `data` tree or the `Primary` lighting block is new. The two new things are a `sHash` on the module, and the signing step that turns the plain `signed-stool-and-bucket.json` into the signed `signed-stool-and-bucket.msf`. Everything about the scene itself - `Container`, the `Primary` lighting, the `Children` tree, `Transform`, and the spot lights - works exactly as [Example 02](../02-stool-and-bucket/README.md) explained.

## The files


| File                                    | What it is                                                                                                                   |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `signed-stool-and-bucket.json`          | The payload: the plain fabric you write and then sign. Same scene as Example 02, plus a module hash.                         |
| `signed-stool-and-bucket.msf`           | The signed fabric: the payload wrapped with a signature and the author's certificate. This is the file you actually publish. |
| `wasm/map.wasm`                         | The stock module, shared with the other examples.                                                                            |
| `assets/Stool.glb`, `assets/Bucket.glb` | The models, reused from Examples 01 and 02.                                                                                  |


