---
layout: page
title: The Arena
collection: projects
cover: /assets/the-arena/cover.jpg
---

## Introduction

The Arena is a small videogame developed just by me during a University Course called "Interactive Virtual Environment and Videogames" I attended in 2012 in Salerno.

Even though it is far from being a polished product, I am attached to it as it was my very first C++ project and includes some clever solution for the CPU AI.

It uses [Ogre](https://www.ogre3d.org/) for the graphics and [FMOD Ex](https://www.fmod.com/) for the audio.

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/h3naxygcbi0?si=kn6iTapqUmWZ4vFw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## How to play

The player controls a soldier armed with a rifle. He must survive in a small, closed arena where he will face a soldier controlled by the CPU. If the player defeats the opponent, he will pass on to the next *round*, and will be granted an experience point that he can spend to improve one of his statistics:

- power: increases the amount of damage dealt by a rifle shot;
- fire rate: decreases the interval between two consecutive shots;
- health: increases player's maximum health;
- speed: increases player's movement speed.

While the player becomes stronger as the rounds go on, the opponent also increases his statistics and also becomes more accurate at shooting.

### Take cover!

The enemy will try to take cover behind obstacles as much as possible. Since rifles have a low fire rate, taking advantage of covers is fundamental is the players want to advance the game!

## Artificial intelligence

The Arena was a solo project. I soon discovered that there was simply no time to make a story-based game and that I had absolutely no skills to make enjoyable graphics... so I had to focus on game-play and AI.

### The grid

The game field is basically a rectangular room with obstacles in it. Obstacles are columns or straight walls. Thus, the field can be represented as a grid whose cells can be empty, occupied by an obstacle or by a soldier.

The game AI works on the assumptions that obstacles can't be concave: once we know the coordinates of the four vertices of an obstacle, we know that a soldier can't in the space enclosed in them.

### Path finding

Given the assumptions above, the game implements a fast custom algorithm instead of A*.

As an example, look at the image below. Let's say the opponent wants to move from the blue cell to the green square. The path finding algorithm (PFA) will analyze the grid and build a path (i.e. a sequence of way-points to reach in a given order) from the starting cell to the destination cell.

![Obstacle avoidance](/assets/the-arena/obstacle-avoidance.png){:.center}

The first phase of PFA consists of finding which obstacles must be avoided. Using the [Brasenham algorithm](http://playtechs.blogspot.it/2007/03/raytracing-on-grid.html), the PFA traces an imaginary line between the starting and destination cell and checks cells that are *touched* by that line. If obstacles are present in cells touched by the line, those obstacles are placed in a obstacles list, ordered by distance from the starting cell.

In our example, the obstacles list is: [2, 5].

While the obstacles list is not empty, the PFA:

1. Extracts the first obstacle from the list (the closest one from the last way-point added to the path).
2. Takes the obstacle's vertex V<sub>1</sub> that is closest to the last way-point added to the path and adds V<sub>1</sub> to the path.
3. Takes the obstacle's vertex V<sub>2</sub> that is adjacent to V<sub>1</sub> and that is closest to the destination cell and adds V<sub>2</sub> to the path.

Once the obstacles list is empty, the PFA adds the destination coordinates to the path.

![Path selection](/assets/the-arena/path-selection.png){:.center}

The image above shows the resulting path in red. A couple of consideration:

- The PFA constrains the opponent to move alongside obstacles as much as possible. This is, of course, the desired behavior since the soldier's goal is to stay behind obstacles to take cover.
- The PFA does not necessarily return the shortest path. This is not really a problem: the goal of the soldier is to stay covered. It may happen sometimes that the PFA builds a clearly wrong path, but it is very unlikely for the player to find out in the heat of the game.

#### A note on random positions

The opponent will move to a randomly selected position in the arena while he's looking for the player or when he's trying to flee and take cover.

What if the randomly selected position is occupied by an obstacle or by the player? Instead of checking for collisions, the PFA checks the grid.

- If the selected position belongs to a cell occupied by an obstacle, randomly choose a vertex of the obstacle and use it as the new destination.
- If the selected position belongs to a cell occupied by the player, just begin moving to it. When the player is closer enough, the opponent AI will stop and begin shooting or try to take cover. 

### Obstacle avoidance

Obstacle avoidance can be an intensive task for the game engine, as it needs to check vertex collisions.

#### How to the opponent avoids obstacles?

He doesn't.

Obstacles are unmovable and the PFA will make sure the opponent will never try to go through an obstacle.

#### How the opponent avoids the player?

He doesn't!

When the player gets closer enough to the opponent, the opponent will stop running and start shooting. If the player gets even closer, the opponent will flee to a covering position in opposite direction of the player (see *Cover system*).

#### How the player avoids obstacles and the opponent?

The grid allows quickening spatial queries without the use of Quad Trees. Obstacle avoidance algorithm is only executed between the player model and obstacles (or opponent) found in surrounding cells.

### Cover system

The opponent will try to stay behind obstacles as much as possible (and the player should do the same).

Let's see how the cover system is implemented. Look at the image below. The enemy (blue dot) spots the player (red dot) and decides to take cover instead of firing at him in the open field. 

![Cover system](/assets/the-arena/cover-system.png){:.center}

The Cover Algorithm (CA) will first pick an obstacle to use as cover. It chooses the closest obstacle to the opponent (who want to hide as soon as possible, of course). In our example, the opponent will run for obstacle 1.

Then, the CA traces an imaginary line between the player position and the selected obstacle's center. The hiding position will be a point on that line that is on the opposite side of the player (the green dot in the image below).

The CA has decided the destination point and calls the PFA to make the opponent move to it.

## Controls

- W, A, X, D: move/strife;
- SHIFT: run;
- Mouse: look;
- Left mouse button: shoot.

## Download

The Arena requires at least Windows XP with DirectX 9.0c.

![CC-BY-NC-ND-3.0](/assets/cc-by-nc-nd-30-88x31.png){:.center}

The Arena by Luca Vicidomini is licensed under a \
[Creative Commons Attribuzione - Non commerciale - Non opere derivate 3.0 Unported License](http://creativecommons.org/licenses/by-nc-nd/3.0/deed.it)
{:.text-center}

[Download The Arena](https://www.dropbox.com/scl/fi/kpdicbeugdtmj64wgnb9d/TheArenaSetup.msi?rlkey=hrom2mwnidq7tiob17o8eg6os&st=gn6qq0a0&dl=0)
{:.text-center}