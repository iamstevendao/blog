---
layout: post
title:  "Use Dijkstra's Algorithm in Pacman"
date:   2017-09-12 15:02:48 +1000
categories: algorithm
---
This is how my pacman game looks like:  
![ingame](https://thumbs.gfycat.com/FantasticFondBarnowl-size_restricted.gif)

It had been gone through a painful road to reach this beautiful destination.
I am telling about that road, it can be splitted into 4 bullets:

- [Ghosts run randomly through the map](#randomly-through-map)
- [Ghosts chase Pacman with the shortest path](#chase-pacman-with-the-shortest-path)
- [Ghosts chase Pacman with different paths](#chase-pacman-with-different-paths)
- [Ghosts chase randomly among Pacman and Cherries](#chase-randomly)

## Randomly through map
It was a bit tricky at my first approach, since every time HTML gets redraw using canvas, the ghosts don't move from 1 block to another block but 1/4 block to make game smooth. So they shouldn't seach for a random direction every step they take. But the range for them to move in a 90 degree angle is bigger than the block because it uses the same logic when user tries to control pacman:

Because it randoms a new direction when it is able to turn into other sides but not the way back except when it can't move forward, when it turns right, in the first frame after that, technically it still can turn into the direction before. So sometimes when it turns left or right, it keeps moving forward and backward.
My friend, Chris said that "why don't let the ghost finished one block before thinks about changing its direction", it was not my solution at the end of the day but it made me realize that it would be something really close to the solution.

**Finally, ghosts only random a new direction when its coordinates are integer.**

## Chase Pacman with the shortest path

## Chase Pacman with different paths

## Chase randomly