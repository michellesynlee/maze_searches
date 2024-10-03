# maze_searches

#Assignment for AI class!
#Practice on Breadth First Search, Depth First Search, A* Search, and IDA* Search with DFS using queues/stacks, arena.txt shows examples of mazes that are solved and return a path from start 's' to 'g' 

#Actual assignment instructions:
I. Introduction
The aim of this homework is to build a search agent for a robotic path planning. You will be implementing and comparing several search algorithms and evaluating their performances. More specifically, we would like to build Mark1, a search agent robot for our AI class. We want Mark1 to be able to navigate in a given map with obstacles. The first important task of the robot is assigned to you. To make crafting this first prototype easier and manageable, the robot leader has allowed for various assumptions to hold in the map and the robot. These are described as follows:
Assumptions:
1. The map is a rectangular arena of size m × n which is bounded by walls on the four sides.
2. An obstacle in the map is marked by an “o”, and an empty positions is marked by “ ” (blank space) in the
map.
3. The robot always starts from one exact position marked “s” in the map.
4. The robot has to reach one and only one goal position marked “g” in the map. The goal position is guaranteed to be reachable from the start position.
5. The robot is allowed to move only in one of the four directions (UP, RIGHT, DOWN and LEFT), one move at a time.
6. The cost of moving from one point to any neighbour is the same and equal to one. Thus, the total cost of path is equal to the number of moves made from start to the goal position.
7. Though the robot is autonomous, it is assumed that the robot is always aware of the complete map and its current location w.r.t to the map at anytime.
NOTE: This example diagram is intended to illustrate the format of the input maze and the expected output maze only and may or may not represent a solution from any of the expected implementations
 Page 2
II. Algorithm Review
Recall from lecture that search begins by visiting the root node of the search tree, given by the initial state. Three main events occur when visiting a node:
• First, we remove a node from the frontier set.
• Second, we check if this node matches the goal state.
• If not, we then expand the node. To expand a node, we generate all of its immediate successors and add them to the frontier, if they (i) are not yet already in the frontier, and (ii) have not been visited yet.
• Nodes MUST BE expanded in the order Up, Right, Down, Left as is implemented in the expand method of the MazeState class of the pseudocode. If this order is not followed, your solution may differ from the expected solution and the autograder will mark your solution as incorrect. Be careful when implementing any method that uses a stack or LIFO queue.
This describes the life cycle of a visit, and is the basic order of operations for search agents in this assignment–(1) remove, (2) check, and (3) expand. We will implement the assignment algorithms as described here. Please refer to lecture notes for further details, and review the lecture pseudo-code before you begin.
IMPORTANT: You may encounter implementations that attempt to short-circuit this order by performing the goal-check on successor nodes immediately upon expansion of a parent node. For example, Russell & Norvig’s implementation of BFS does precisely this. Doing so may lead to edge- case gains in efficiency, but do not alter the general characteristics of complexity and optimality for each method. For simplicity and grading purposes in this assignment, do not make such modifications to algorithms learned in lecture.
III. What You Need To Submit
Your job in this assignment is to write maze.py, which solves any maze using the graph search method(s) passed in as flags. The program will be executed as follows:
$python3 maze.py -m <map filename.txt> [flags]
The flags will be one or more of the following (you will have implemented all of them by the end of the assignment): -bfs (Breadth-First Search)
-dfs (Depth-First Search)
-astar (A-Star Search)
-ida (Iterative Deepening A-Star Search) -all (Run all 4 search algorithms)
Some sample execution commands are shown below:
$python3 maze.py -m arena1.txt -bfs
$python3 maze.py -m arena2.txt -dfs -astar -ida $python3 maze.py -m arena3.txt -all
Note: Most of the code to format the output has been provided in the skeleton. You simply need to write the bfs, dfs, astar, ida functions along with some of the functions in the MazeState class. Writing additional helper functions is highly recommended but optional.
Page 3

IV. What Your Program Outputs
Your program will write the following for each graph search algorithm requested in the flags to the stdout:
path to goal: the path taken by the robot shown as “*” on the graph
cost: the number of steps taken to reach the goal
nodes expanded: the number of nodes that have been expanded
max nodes stored: the maximum number of nodes stored in the frontier set during the runtime of the algorithm max search depth: the maximum depth of the search tree in the lifetime of the algorithm
running time: the total running time of the search instance, reported in seconds
max ram usage: the maximum RAM usage in the lifetime of the process as measured by the ru maxrss attribute in the resource module, reported in kilobytes.
Your code will only be tested directly on the bfs, dfs, astar, ida functions. The input to each of these functions is an array of strings in which each string represents a row of the maze. The expected output is the variables listed above in that order. The path to goal is returned as an array of strings in the same format as the input, except with the solution path traced with “*”. The other variables are returned as integers or floating point numbers.
As long as these 4 functions return the correct output for any given input, you should receive a full score.
Note on Correctness
All variables, except running time and max ram usage, have one and only one correct answer when running BFS and DFS. A* and Iterative Deepening A* nodes expanded, max nodes stored, and max search depth might vary depending on implementation details. You’ll be fine as long as your algorithm follows all specifications listed in these instructions.
As running time and max ram usage values vary greatly depending on your machine and implementation details, there is no “correct” value to look for. They are for you to monitor time and space complexity of your code, which we highly recommend. running time and max ram usage MUST be returned even if we do not grade them. If you do not return these values for a test case, you may receive a 0 for that test case.
A good way to check the correctness of your program is to walk through small examples by hand, like the ones above. Use the following piece of code to calculate max ram usage:
import resource
dfs start ram = resource.getrusage(resource.RUSAGESELF).ru maxrss
dfs ram = ( resource . getrusage ( resource .RUSAGE SELF). ru maxrss − dfs start ram )/(2∗∗10)
Our grading script is working on a linux environment. For windows users, you may change max ram usage calculation code so it is linux compatible during submission (You can test you code on linux platforrm using services such as Google Colab). However, since the value of max ram usage will not be tested, this step is not necessary.
 Page 4

V. Implementation and Testing
For your first programming project, we are providing hints and explicit instructions. Before posting a question on the discussion board, make sure your question is not already answered here or in the FAQs.
1. Implementation
You will implement the following four algorithms as demonstrated in lecture. In particular:
• Breadth-First Search. Use an explicit queue, as shown in lecture.
• Depth-First Search. Use an explicit stack, as shown in lecture.
• A-Star Search. Use a priority queue, as shown in lecture. For the choice of heuristic, use the Manhattan distance; that is, the l-1 distance between the current position and the goal positions.
Note: It may be helpful here to modify the PuzzleState object to allow for comparison between 2 states. You can explore pythons eq , hash and lt methods or other such methods for this purpose. Implementations of the eq and hash methods have been provided for you, but you may modify them.
• Iterative Deepening A-Star Search. It is recommended that you use a recursive Depth Limited Search helper function. For the choice of heuristic, use the Manhattan distance. As an additional minor optimization, you may think about what the shortest possible solution for a given maze could be and start there instead of starting with a depth limit of 1.
2. Order of Visits
In this assignment, where an arbitrary choice must be made, we always visit child nodes in the “URDL” order; that is, [‘Up’, ‘Right’, ‘Down’, ‘Left’] in that exact order. Specifically:
• Breadth-First Search. Enqueue in URDL order; de-queuing results in URDL order.
• Depth-First Search. Push onto the stack in reverse-URDL order; popping off results in URDL order.
• A-Star Search. Since you are using a priority queue, what happens with duplicate keys? How do you ensure nodes are retrieved from the priority queue in the desired order?
• Iterative Deepening A-Star Search. When performing regular A-Star Search, we do not revisit nodes. Is that the case with Iterative Deepening A-Star Search as well? Or do we need to revisit nodes to ensure an optimal solution?
Ensure your algorithm starts at the start node and ends at the goal to avoid alternate solutions which will be marked as incorrect by the autograder.

