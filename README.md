# ProtectTheFlame
**Unity Game Path Finding**

Span of Project: 6/10-24/18

**How To Play**
1. Download the .rar file.
2. Run the .exe file.
3. Q/E to rotate
4. Space to shoot
5. 1/2 to switch between fire and hook mode

**Game Background**

tl;dr: protect a fire spirit, rotate around the spirit to shoot alive enemies or hook dead enemies, buy upgrades in a shop

  The player is a protective guardian that rotates around a friendly fire spirit. Enemies will spawn randomly around the screen and walk towards the flame. The player needs to shoot fireballs to kill the enemies and protect the fire spirit. Shooting will drain the fire's vitality by a little, but enemy contact with the fire will deplete the flame's strength significantly. The player can restore the fire's strength by using hooking dead enemy corpses into the fire. Hooking a dead body will give the player a Bone, which is the game's currency. Ocasionally, a truck will enter the screen that offers the players items at the price of Bones. These items include rock obstacles, player upgrades, traps, and friendly minions. 

**Script Accomplishments**

tl;dr: made custom path finding algorithm for enemies to detour around obstacles

  Probably the hardest part of this project was making the enemy path finding system. At first, I had a simple pathing system where the enemies would just walk directly at the fire in a direct and unelegant fashion. This served its purpose well: to give the enemies a target to threaten. However, I wanted to add the obstacle feature where players could place blocks and traffic the enemies through a specific opening. If I were to maintain the old pathing system where enemies would simply walk towards the fire, they would get stuck at an obstacle and endlessly walk into a wall. I had to revamp the pathing script and give the enemies a way to navigate around obstacles.
  
  To do this, I first created a grid of square blocks to make up the "floor" of the game. With these individual nodes, I could now gather nodes to form a path, store the nodes into a list, and feed the list to an enemy instance to traverse. In a simple case where there are no obstacles, a path can be formed by drawing a line from the enemy spawn point to the fire. Any floor nodes that intersect with the line will be included in a list. The list will then be sorted based on the node's distance from the fire in decreasing order. This means that the node farthest from the fire will be first on the list and the node closest to the fire will be last on the list. By feeding an enemy instance this list, it can now traverse the list node by node like a series of instructions. Right now the enemy can walk towards the fire in an organized fashion, but it still doesn't solve the obstacle problem. To navigate around an obstacle, I needed to first define a detour route. When an obstacle is placed, it creates a circularly linked list of the floor nodes surrounding it. Writing this script had its own complications(resolving cases of merging obstacles, finding a node's correct neighbor, preventing node retracing), but everything worked out in the end. 
  
  Having the two tools constructed and refined, it was time to implement the detour system into the straight line path. I came up with an analogy while working on this part. In the analogy, the straight line path is a bullet, the obstacle is a human body, and the detour system is the circumference of a human body's waist. When a bullet enters a human body, it leaves an open wound, and when the bullet exits the body, it leaves an exit wound. Both the open and exit wound is guaranteed to be within the circumference of the waist. Applying this analogy, the straight line path will share exactly 2 floor nodes with the obstacle's detour list. These two floor nodes represent the open and closing wound. To navigate around the obstacle, we just need to take a subset from the detour list from the open wound to the exit wound and insert that subset between the open and closing wound for the straight line path. Enemies would now reach the obstacle's open wound, detour around the obstacle until it reaches the closing wound, and then continue its straight line path. 
