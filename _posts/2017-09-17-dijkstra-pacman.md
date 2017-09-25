---
layout: post
title:  "Dijkstra's Algorithm for Pacman"
date:   2017-09-17 23:59:59 +1000
categories: algorithm
---
This is how my pacman game looks like (checkout the [demo](https://repl.it/Jv9c/34) or [source](https://github.com/iamstevendao/pacman)):  
<p align="center">
<img src="https://thumbs.gfycat.com/FantasticFondBarnowl-size_restricted.gif"/>
</p>
It had been gone through a painful road to reach this beautiful destination.
I am telling about that road, it can be splitted into 4 bullets:

- [Ghosts run randomly through the map](#randomly-through-map)
- [Ghosts chase Pacman with the shortest path](#chase-pacman-with-the-shortest-path)
- [Ghosts chase Pacman with different paths](#chase-pacman-with-different-paths)
- [Ghosts chase randomly among Pacman and Cherries](#chase-randomly)

## Randomly through map
It was a bit tricky at my first approach, since every time HTML gets redraw using canvas, the ghosts don't move from 1 block to another block but 1/4 block to make game smooth. So they shouldn't seach for a random direction every step they take. But the range for them to move in a 90 degree angle is bigger than the block because it uses the same logic when user tries to control pacman:

<p align="center">
<img src="https://odxwwq.bn1302.livefilestore.com/y4mzvZvsn6p_qk82jNKB2BUNtKvmRZINjDJdjLxTOiNo43jORy-JH026E0bS-vL1TmyB_VCjbexNtERU6rkoIT8WMrlF23Jrc5g76RLL8cFqjvhnRC0Md-YPfNbBB77W4BXc80qBljdBd5YkDGmDYap9_S0UOpBnRaB-e0kxd_ec7w6IdpMkxMriUAqMxnfjcE7pVbYEzVNgDJ5Ral_ePM3oA?width=516&height=273&cropmode=none" alt="character-turns"/></p>
Because it randoms a new direction when it is able to turn into an other side but not the way back except when it can't move forward, when it turns right, in the first frame after that, technically it still be able to turn into the previous direction. So sometimes when it turns left or right, it keeps moving forward and backward.
My friend, Chris said that "why don't let the ghost finished one block before thinks about changing its direction", it was not my solution at the end of the day but it made me realize that it would be something really close to the solution:

```js
//if the ghost is at the center of a block, random a new direction
//otherwise do nothing
if (Number.isInteger(obj.x) && Number.isInteger(obj.y)) {
	obj.direction = possibleDirection[Math.floor(Math.random() * possibleDirection.length];
}
```
**Finally, ghosts only random a new direction when its coordinates are integers.**

## Chase Pacman with the shortest path
After reading a lot of articles about Dijkstra's Algorithm and other shortest path finding algorithms, I decided to implement one of them in my game.
It would be a bit different since Pacman doesn't have nodes (or vertices) and it is not a good solution to value every corners, intersection with numbers. While stucking a while, I saw this algorithm in [Dijkstra's Algorithm Wikipedia page](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm):

<p align="center">
<img src="https://upload.wikimedia.org/wikipedia/commons/2/23/Dijkstras_progress_animation.gif"/>
</p>

it seems to be exactly what I was looking for, this is how the code looks like:

```js
function findWay(ind, departure, destination) {
		//if arrival and departure have a same coordinates, return a blank array
		if (departure.x == destination.x && departure.y == destination.y)
			return [];
		//push departure to the queue as the start point
		let queue = [departure], index = 0, result = null;

		//keep finding a way until get a result
		while (result == null) {
			let adj = getAdjacences(ind, queue, queue[index]);
			adj.forEach((value) => {
				//deep copy the adjacence and push to queue
				value.prev = JSON.parse(JSON.stringify(queue[index]));
				queue.push(value);

				//if it reaches its target, assign to result and break the loop
				if (value.x == destination.x && value.y == destination.y) {
					result = value;
				}
			});
			index++;
		}

		//inverse nextMoves
		let nextMoves = [];
		let curr = result;
		do {
			nextMoves.push({ x: curr.x, y: curr.y });
			curr = curr.prev;
		} while (curr != null);

    //return nextMoves without the first one (the ghost itself)
		return nextMoves.reverse().splice(1);
	}
```
Basically it keeps pushing adjacences (4 directions beside) of all current points to the queue, until it meets Pacman.
But the problem now is, after a very small amount of time, all ghosts go on a same way to chase Pacman, because the starting points of ghosts are *kinda* near to each other (inside a 5*3 block in the center of the map).

## Chase Pacman with different paths
I know that obviously there are several solution for this new problem, since I found [K shortest path routing algorithm](https://en.wikipedia.org/wiki/K_shortest_path_routing) immediately.
However, instead of implementing a new algorithm, and being lazy of reading, I decided to modify a bit in my ***getAdjacences()*** function, by adding ***shuffle()*** to 4 adjacences of every point on the way to Pacman.

At first:
```js
adj = [{ x: point.x, y: point.y - 1 }, 
      { x: point.x, y: point.y + 1 }, 
      { x: point.x - 1, y: point.y }, 
      { x: point.x + 1, y: point.y }]
```
after changes:
```js
let adj = shuffle(ind, 
                [{ x: point.x, y: point.y - 1 }, 
                { x: point.x, y: point.y + 1 }, 
                { x: point.x - 1, y: point.y }, 
                { x: point.x + 1, y: point.y }]
                );
```
with ***ind*** is the index of the current ghost in the list, to keep it shuffle for one and only direction during its search.

So now ghosts will chase Pacman by *different* shortest path (eventhough they could be  same if the path is too explicit).

I was not satified yet, it is a bit weird if all the ghosts keep chasing Pacman the whole game, and it only increased the difficulty of the game a bit. And I came up with an idea, what about cherries, Pacman tends to get cherries to eat ghosts and finish the game, so why don't some ghosts instead of chasing Pacman, chase among cherries?

## Chase randomly
It was not hard when being at this point.
Firstly, I need a random, for each ghost will have **80%** of chasing Pacman and **20%** of chasing cherries (if there are cherries left on the map):

```js
function generateTarget(obj) {
	obj.path = []; //clear current path of course
	if (cherries.length > 0) // if there are cherries on the map
		obj.target = Math.floor(Math.random() * NUMBER.GHOST) > NUMBER.GHOST / 4 ? -1 : Math.floor(Math.random() * cherries.length);
	else
		obj.target = -1;// -1 means pacman, others mean the index of cherries in the list
}
```

And to make it even more effective, ghosts generate a new target every 20s. >.<

One more thing to end this post, the ghost will go backward and randomly when Pacman eats a cherry, easily by inverse its direction at that moment and stop searching for new path until Pacman is back to normal.