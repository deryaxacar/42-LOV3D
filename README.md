<!-- Project Title -->
<h1 align="center"> 42 - cub3d 🕹️</h1>

<!-- Project Description -->
<p align="center">
The Cub3d project aims to create a 3D labyrinth game. The player navigates through the maze from a first-person perspective, trying to reach certain goals. This project covers fundamental aspects of game programming, graphics management, and user interaction. Furthermore, it provides experience in basic 3D game mechanics, collision detection, and event management.
</p>

<!-- Project Logo or Image -->
<p align="center">
  <a target="blank"><img src="https://i.hizliresim.com/lc3txl2.png?_gl=1*1ia0uuv*_ga*MTg2MDIyNTgxMC4xNzM0ODc2OTYy*_ga_M9ZRXYS2YN*MTczNDg3Njk2MS4xLjEuMTczNDg3Njk3Ny40NC4wLjA." height="150" width="150" /></a>
</p>

---

## Project Purpose 🎯

The purpose of the Cub3d project is to develop a 3D maze game played from a first-person perspective. In the game, the user controls a character moving through a labyrinth to accomplish certain objectives. This project focuses on 3D project structure, game mechanics, graphics management, and user interaction.

The labyrinth is built from a series of map files, and the character moves around this environment based on user input. This project aims to develop essential game programming skills such as collision detection, event handling, and user interactions.

### Core Objectives 🏆

- **Maze Creation (3D):** Read map files in a 3D environment and render them correctly on the screen. The map files, which include walls, passable areas, and goal points, define both the visual layout and functionality of the game. 🗺️
- **Character Movement:** Allow the user to move the character freely from a first-person perspective using arrow keys or similar controls. This movement is crucial for smooth gameplay and interactivity. 🕹️
- **Collision Detection:** Detect collisions between the character and walls or other objects. This ensures that impassable areas are respected and that game physics are applied. 🚧
- **Game Events:** Manage events such as game start, end, and various user interactions. These include game flow, player feedback, level transitions, and completing objectives. 🎮
- **User Interaction:** Optimize the user interface and control mechanisms so that the player can easily adapt to the game and have an enjoyable experience. 🖱️
- **Game Performance:** Improve the overall performance and efficiency of the game. Smooth graphics rendering and event handling enhance both the technical quality of the game and the player’s experience. ⚡

## Ray Casting Algorithm 🌍

---

<p align="center">
  <a target="blank"><img src="https://i.hizliresim.com/o145kzp.PNG?_gl=1*1act2a9*_ga*MTg2MDIyNTgxMC4xNzM0ODc2OTYy*_ga_M9ZRXYS2YN*MTczNDg3Njk2MS4xLjEuMTczNDg3NzkwNC41OS4wLjA." /></a>
</p>

---

The fundamental method used to achieve the 3D labyrinth view in the Cub3d project is the **Ray Casting** algorithm. This algorithm transforms a 2D map into a first-person 3D view.

**Ray Casting** principles:
1. The player has a specific position (x, y) and viewing angle (angle) within the game world.
2. For each vertical “pixel column” on the screen, a “ray” is cast according to the player’s viewing angle.
3. The ray is extended until it intersects with a wall or obstacle on the map.
4. The point where the ray first intersects a wall determines the wall height to be drawn in that column.
5. By calculating wall heights for every column, a 3D perspective is simulated on the screen.

Below is an example **C** function demonstrating a simple skeleton of the ray casting logic:

```c
#include <math.h>
#include <stdio.h>

#define FOV 60.0      // Player's field of view in degrees
#define SCREEN_WIDTH 640
#define SCREEN_HEIGHT 480

// Map definition (0 -> empty space, 1 -> wall)
int map[8][8] = {
    {1,1,1,1,1,1,1,1},
    {1,0,0,0,0,0,0,1},
    {1,0,1,0,0,0,0,1},
    {1,0,0,0,0,1,0,1},
    {1,0,0,0,0,0,0,1},
    {1,0,0,1,0,0,0,1},
    {1,0,0,0,0,0,0,1},
    {1,1,1,1,1,1,1,1},
};

// Player initial position
float playerX = 3.5, playerY = 3.5;
float playerAngle = 0.0;

void cast_rays(void)
{
    // Cast a ray for each pixel column on the screen
    for (int x = 0; x < SCREEN_WIDTH; x++) {
        // Map the x column on screen to angles between -FOV/2 and +FOV/2
        float rayAngle = (playerAngle - (FOV / 2.0)) + ((float)x / SCREEN_WIDTH) * FOV;
        
        // Convert degrees to radians
        float rayRad = rayAngle * M_PI / 180.0;

        // Ray marching
        float stepSize = 0.1;  // Ray step size
        float distance = 0.0;  // Distance to wall
        float hitX = playerX;
        float hitY = playerY;
        
        int hitWall = 0;

        // Advance the ray until it hits a wall or reaches the map boundary
        while (!hitWall && distance < 20.0) {  // 20 -> example maximum view distance
            distance += stepSize;
            hitX = playerX + cos(rayRad) * distance;
            hitY = playerY + sin(rayRad) * distance;

            // Check map boundaries
            if (hitX < 0 || hitX >= 8 || hitY < 0 || hitY >= 8) {
                hitWall = 1;
                distance = 20.0;  // Treat this as if a wall is hit
            } else if (map[(int)hitY][(int)hitX] == 1) {
                hitWall = 1;  // A wall is found
            }
        }

        // Calculate wall height (simple approach)
        int wallHeight = (int)(SCREEN_HEIGHT / distance);
        
        // Here, you could draw the wall for this column:
        // draw_vertical_line(x, wallHeight); // e.g., a custom drawing function
    }
}

```
Bu örnek, ray casting mantığını çok basite indirgenmiş şekilde anlatmaktadır. Gerçekte, duvar çizimi ve texture mapping gibi ek işlemler yaparak daha detaylı bir görüntü elde edebilirsiniz.

## Tools Used 🛠️

Key tools and libraries used in the Cub3d project include:
- **MiniLibX:** A library for graphics operations and window management.
- **Xlib:** A library for interacting with the X Window System.
Requirements 📋

## Requirements 📋

To run the Cub3d project, you need to meet the following requirements:

- A Unix-based operating system (Linux, macOS)
- GCC compiler
- MiniLibX library

---
