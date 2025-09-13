---
layout: page
title: Mini Minecraft
description: 
img: assets/img/proj5600/mnm_preview.png
redirect: 
importance: 3
category: fun
---

The overall goal of this project is to create an interactive 3D world exploration and alteration program in the style of the popular computer game Minecraft. Implemented features include random terrain generation, vegetation generation matched to the ecosystem, character physics-based motion, and character–object interactions within the scene.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/MiniMinecraft.mp4" class="img-fluid rounded z-depth-1" controls=true autoplay=true %}
    </div>
</div>


### Efficient Terrain Rendering and Chunking

A `Chunk` is a simple data structure that stores a 16 x 256 x 16 collection of `BlockType`s in a 1D array, and a set of pointers to its four neighboring `Chunk`s in the + and - X and Z directions. Presently, the base code renders all of the blocks in all of the `Chunk`s by drawing the same basic `Cube` mesh over and over again with a different model matrix applied each time, once per block in the terrain. Since this amounts to hundreds of draw calls per frame, this is very inefficient. 



When we implement the `create()` function for our `Chunk` class, we should only create VBO data for block **faces** that lie on the boundary between an `EMPTY` block and a filled block. In the diagrams below of a 2D Minecraft setup, four blocks have been filled in. However, only the ten block sides that are on the interface between a filled block and empty block are rendered.



### Game Engine Tick Function and Player Physics

In Mini Minecraft, we use the `MyGL` class as our "game engine" construct, meaning `MyGL` will eventually need to know about all entities of which the game comprises and will need to handle processing each entity each time the game updates.

The `Entity` class is created to represent any object in the Minecraft game world that is not part of the terrain, such as the `Player`, any `Camera`. Every `Entity` keeps track of its own local coordinate system, represented by its own world-space position and axes defined relative to the world-space axes. The `Entity` class also comes with functions to enable easy movement along its local axes as well as the world-space axes. Importantly, the `Entity` class declares, but does not define, a `tick` function, which will be implemented by any classes that inherit from it. This function will encompass any actions an `Entity` needs to take every frame.

A `Player` class is provided that inherits from `Entity`. This will represent the player's avatar in the Minecraft virtual world. In addition to the members inherited from `Entity`, the `Player` stores a `Camera` (which also inherits from `Entity`) to represent the player's point of view and render the game world, tracks its velocity and acceleration for smoother physics, and has a handle to the `Terrain` so the `Player` can interact with it.

The game follows the following operations:

- The player begins the game in "flight mode", which allows them to move without being subject to gravity or terrain collisions
- In flight mode, the following behavior occurs based on which keys are currently pressed down:
- W -> Accelerate positively along forward vector
- S -> Accelerate negatively along forward vector
- D -> Accelerate positively along right vector
- A -> Accelerate negatively along right vector
- E -> Accelerate positively along up vector
- Q -> Accelerate negatively along up vector
- F -> Toggle flight mode OFF
- When flight mode is not active, the player is subject to gravity and terrain collisions (described later). Additionally, the player's movement changes slightly:
- W -> Accelerate positively along forward vector, discarding Y component and re-normalizing
- S -> Accelerate negatively along forward vector, discarding Y component and re-normalizing
- D -> Accelerate positively along right vector, discarding Y component and re-normalizing
- A -> Accelerate negatively along right vector, discarding Y component and re-normalizing
- Spacebar -> Add a vertical component to the player's velocity to make them jump
- F -> Toggle flight mode ON
- In both movement modes, the player's velocity is reduced to less than 100% of its current value every frame (simulates friction + drag) before acceleration is added to it.

In order to collide with terrain, we assume that the `Player`'s collision volume is in the shape of two Minecraft blocks stacked on top of one another, always aligned to the world axes, with the `Player`'s position sitting at the bottom-center of the lower block. The `Camera` controlled by the `Player` always sits 1.5 units above the `Player`'s position, placing it in the center of the top cube (the player's "head").



<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/proj5600/player_bounds.png" title="player bounds" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Player bounds
</div>

### Texturing and Texture Animation

Using the `BlockType` of a given block, set the UV coordinates of a square face in the `Chunk` VBO so that they correspond to the appropriate texture square in the image below. For example, if you were adding the VBO data for the top of a `GRASS` square, then you'd set the square's UVs so they aligned with the texture area labeled `GRASS TOP` (see image below). If you want to test block appearances, you might consider setting keyboard input that has the Player place a particular block in front of them. Remember that UV coordinates have a range of 0 to 1; the image below is divided into 16x16 block faces, so a single block face ranges from `<x, y>` to `<x+1/16, y+1/16>`.

When a block is a `LAVA` or `WATER` block, its UVs should be offset in your GLSL shader based on an incrementing time variable. Note that there are rows of `WATER` and `LAVA` face textures in the provided texture image; you can repeatedly move the UVs of a `WATER` block or `LAVA` block across this row to give the illusion of moving fluid across the block surface.

These tell the OpenGL pipeline that it should use the alpha channel (i.e. the W coordinate) of a fragment's color `vec4` to blend that fragment with the other fragments for its pixel. An alpha of 1 means the fragment is fully opaque, while an alpha of 0 means the fragment is fully transparent. Note that the order in which triangles are rendered will affect how the alpha blending appears, but we aren't going to worry about that for this milestone. 



<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/proj5600/minecraft_textures_all_labeled.png" title="player bounds" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Player bounds
</div>
