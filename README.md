# Maze Escape

A 3D maze navigation game built in C++ with OpenGL/GLUT. Players race against the clock to navigate a randomly generated 3D maze, reach the exit, and avoid obstacles scattered along the way.

## Overview

Maze Escape generates a new maze on every run using a randomized depth-first carving algorithm, guarantees a solvable path with BFS pathfinding, and places obstacles off that path so the maze always remains beatable. The game is rendered in a 3D perspective view, with a start screen, difficulty selection, an in-game timer, and win/lose/crash end states.

## Features

- **Procedural maze generation** using randomized recursive backtracking, with extra wall openings carved in for varied layouts.
- **Guaranteed solvability** via a breadth-first search (BFS) that computes the shortest path from the player's start position to the exit.
- **Obstacle placement** that avoids the shortest path so every maze remains completable.
- **Three difficulty levels**, each with its own maze size and time limit:

  | Difficulty | Maze Size | Time Limit | Obstacles |
  |---|---|---|---|
  | Easy | 11 x 11 | 60s | 5 |
  | Medium | 15 x 15 | 45s | 10 |
  | Hard | 21 x 21 | 30s | 20 |

- **3D rendering** of walls, floor, player, exit, and obstacles using OpenGL primitives (cubes and spheres) with a top-down perspective camera.
- **Start menu** with clickable rounded buttons for difficulty selection, rendered with custom 2D overlay drawing.
- **Countdown timer** displayed on-screen, tracked via `clock()` and updated every second.
- **Game states:** `START`, `PLAYING`, `WON`, `LOST` (time runs out), and `CRASHED` (player hits an obstacle).
- **Keyboard controls:** WASD or arrow keys to move; `R` to restart and return to the start screen.
- **Mouse controls:** click a difficulty button on the start screen to begin.

## Controls

| Input | Action |
|---|---|
| `W` / Up Arrow | Move forward |
| `S` / Down Arrow | Move backward |
| `A` / Left Arrow | Move left |
| `D` / Right Arrow | Move right |
| `R` | Restart (return to start screen) |
| Mouse click | Select difficulty on the start screen |

## How It Works

### Maze Generation
`carveMaze()` performs a randomized recursive backtracking walk over a grid of odd-indexed cells, shuffling the direction order at each step and knocking down walls between visited cells. A small random chance of carving extra openings adds loops to the maze so it isn't purely a single-path tree.

### Pathfinding
`findShortestPath()` runs a standard BFS from the player's starting cell to the exit, reconstructing the path via a parent map. This shortest path is used purely internally, to keep obstacles off the route the player is guaranteed to be able to walk.

### Obstacles
`addObstacles()` places a difficulty-dependent number of obstacles on open maze cells that are not the player's start, the exit, or any cell on the precomputed shortest path.

### Rendering
The game uses `gluLookAt` for a fixed overhead-angled camera, `glutSolidCube` / `glutSolidSphere` for maze elements, and a 2D orthographic overlay (`gluOrtho2D`) for text and the start-menu buttons, toggling `GL_DEPTH_TEST` off while drawing UI elements.

## Requirements

- C++ compiler with C++17 support (for structured bindings)
- OpenGL
- GLUT (FreeGLUT recommended)

## Building

Example build command on a system with FreeGLUT installed:

```bash
g++ maze_escape.cpp -o maze_escape -lGL -lGLU -lglut
./maze_escape
```

On Windows with Visual Studio, link against `opengl32.lib`, `glu32.lib`, and `freeglut.lib`, and ensure the FreeGLUT headers/DLL are available to the project.

## Project Structure

```
Version 1.cbp   # Single-file implementation: maze generation, pathfinding,
                  # rendering, input handling, and game loop
```

## Contributors

| Name | ID |
|---|---|
| Mahfuza Maisha | C221213 |
| Umme Kawsher | C221231 |
