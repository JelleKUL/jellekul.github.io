---
layout: page
title: Gaussian Splats in Blender
permalink: /development/blendersplats/
---

# Gaussian Splats in Blender — Setup Guide

## 1. Get Blender

https://www.blender.org/download/

## 2. Install the add-on (3DGS Render by KIRI Engine)

Repo: https://github.com/Kiri-Innovation/3dgs-render-blender-addon

1. Open the [Releases page](https://github.com/Kiri-Innovation/3dgs-render-blender-addon/releases) and download the `.zip` matching your Blender version and OS.
2. In Blender: **Edit → Preferences → Get Extensions**.
3. Click the menu in the top-right corner → **Install from Disk**.
4. Select the downloaded `.zip` and confirm.
5. In the **3D Viewport**, press **N**, then select the **3DGS Render** tab.

Guide: https://www.kiriengine.app/3d-tools/3dgs-render Videos: https://www.youtube.com/@3D-Tools-by-KIRI-Engine

## 3. Get a Gaussian Splat `.ply` file

**Option A — Official 3DGS pretrained models (buildings + indoor rooms)** https://repo-sam.inria.fr/fungraph/3d-gaussian-splatting/datasets/pretrained/models.zip

- Truck, Train — outdoor/building scenes
- DrJohnson, Playroom — indoor rooms
- Bicycle, Garden, Room, Counter, Kitchen — mixed indoor/outdoor

Each scene: `point_cloud/iteration_N/point_cloud.ply`

Mirror (drjohnson, playroom, train, truck): https://huggingface.co/datasets/Voxel51/gaussian_splatting

**Option B — Scan your own building/room**

- Polycam: https://poly.cam/
- Luma AI: https://lumalabs.ai/

Both export directly to a `.ply` Gaussian Splat file.

## 4. Import the splat

1. Open the add-on panel (N-panel → **3DGS Render** tab).
2. Set the active mode to **Edit**.
3. Open the **Import** menu and select your `.ply` file.
4. Use **Edit** mode for selections/modifiers, or **Render** mode for output.