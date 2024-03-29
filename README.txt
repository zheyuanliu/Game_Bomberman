Bomberman-Z
Author: Zheyuan Liu
Last Edit: Dec 10, 2013

Basic Game logic:
	This section specifies the relationships among different game objects. 
	In this game, there are mainly five types of objects, the player character, creeps, bombs, explosions, three types of power-ups, and destroyable and non-destroyable blocks. Bombs and blocks will be generally referred to as "unwalkables" in the following paragraphs.
	First of all, a player character can place a bomb based on its location. (Note that the bombs will always be placed in the center of the tile the player character is in.) Once the bombs are placed, they act as unwalkables that cannot be traversed. After 3 seconds of its original placement, a bomb will denotate and turn into an explosion based on the player's current bomb range. An explosion will last roughly 1 second on the ground and will kill any player character or creep, remove power-ups, denotate other bombs, and destroy blocks  that it touches. However, an explosion does not pass through non-destroyable blocks. Any player character must not traverse any unwalkable, including placed bombs and blocks of either type, with the exception of the bomb the player immediately places. A player character has 0.1 - 0.2 second to traverse the bomb immediately after the player places it. However, if for some reason(either because future game modes allow it or simply because the other player places a bomb right below a player) a player is standing on an unwalkable, that player can only walk away from the center of the unwalkable unit based their relative positions. Power-ups hide beneath the destroyable blocks and reveal themselves after the destroyable blocks on top of them are removed. They do not disappear until either a player character walks across it and absorbs it or an explosion removes it. If a player absorbs any powerup, the player's stats gets upgraded. There are three main stats, speed, range, and number of bombs. There are three corresponding types of power-ups as well. However, each stat has its upper limit. (Note that power-ups are randomly placed beneath destroyable blocks.) Lastly, a creep has all the same relationships with other game objects as a player character except---1. its interaction with a player character, which will specified in the Game Modes section; 2. it does not drop bombs.  

Game Modes:
	One-Player:
	The one-player mode is the player vs AI. In this mode, the only way a player can win is to kill all the AI creeps within the three-minute time period. The number of creeps depends on the specific map as the maximum numbers of game characters different maps support are different. A creep roams around randomly across the map, and once a player is in the vincinity of a creep, that creep turns fierce and doubles its speed to follow the player. If a creep touches the player, the player loses. The creeps follows paths generated by the A* algorithm. The player character can outrun the creeps at its maximum speed. A good tip would be to block creeps with bombs and try to kill creeps without them sensing that you are nearby. 

	Two-Player:
	The two-player mode is simple. Two players compete agaisnt each other until the three-minute time period is up. If any player dies, the other player gains one point. After each death, a player has a 5-second death penalty, and upon respawn, a player has a 3-second invulnerability. When the time is up, the player with more points wins. (Note that the tip here is to try to kill the other player without dying yourself.)

Panel Flow:
	 There are six panels in this game, the main menu, the map selection, the help panel, the general info panel, and two game panels. A user enters the main menu when he/she first opens the game. If he/she chooses to see help or general info, the selected panel will show. On the other hand, if he/she chooses either one-player or two-player mode, the map selection will first pop up. The user can preview the maps by clicking the arrow keys. When the user has chosen the map, he/she should select "START" and the selected game will start after 1 second of signature. The "quit" button will take the user back to the main menu and the "ok" button at the end of each game also has the same function.

Class Dependencies:
				     GameObj
	   ---------------------------------------------------------------------
	   |				|                      |               |
 	Player                       Powerup              Unwalkable       Bubble/Bomb
	   |		   --------------------------			       |
	Creep_AI           |       	|	    |			    Explosion
			Speed	     Number 	 Range


				     JPanel
	    -----------------------------------------------------------------------
	    |              |              |                      |                |
	GameMenu	GameHelp    GameCourt_OnePlayer  GameCourt_TwoPlayer  GameMapOpt
			   |
			GameGeneral

	Map

	Animation
	
	Path

	PathFinder

Implementation:
	Dropping Bombs and explosions:
	The map is tile-based. It takes in a valid .txt file to construct maps. For examples, please see Map/map#.txt. The map keeps track of all the explosions, including counting down and removing them. 
	Since the bombs include stats specific to players, the players keep track of their own bombs, including counting them down and "exploding" them. However, the player sends the location and the range of the bomb up to the map which stores them as unwalkables. When the bombs explode, they trigger the explosion method in the map class. 
	
	Pathfinding:
	A huge part of AI is the A* pathfinding algorithm. For a complete tutorial, please visit http://www.policyalmanac.org/games/aStarTutorial.html. The standard distance calculation in this game is the Manhattan Distance method (http://en.wiktionary.org/wiki/Manhattan_distance). The findPath method in the PathFinder generates a Path object specified in the Path class. A path is simple an arraryList with some customized methods that fit the purpose of the AI in this game. AI creeps then follow this path to reach the player character and potential kill it.
	
	Animation:
	All moving objects, including the player characters and the creeps, as well as the flames have animation in this game. Animation is simply achieved through looping in an array of BufferedImage based on a set delay time. In this game, the delay time for the character animation and creep animation varies according to the speed at which they move.

	Powerups：
	The powerups are stored in an arrayList in the map object. When the corresponding blocks are destroyed, the map object draws the powerup objects at the location if there is any. If the player character absorbs any powerup, it removes the powerup in the arrayList.

	Unwalkables:
	All the blocks and bombs are stored in a hashtable that maps their locations to the actual objects. There is no separate class that separates the destroyable blocks and the non-destroyable blocks.
	
	Paint:
	The characters are painted in order of their y positions.

Work Referenced:

A* Pathfinding Algorithm: 
	http://www.policyalmanac.org/games/aStarTutorial.html
	http://www.raywenderlich.com/4946
	http://www.cokeandcode.com/main/tutorials/path-finding/ (Inner class)
Animation: 
	http://www.youtube.com/user/ForeignGuyMike