Hi, I am Andrew Ding.

Several notes:

(1) I tried many other heuristic functions, but many were not useful. The four heuristic functions that I finally 
used were:
positional score: score based off a value map of the squares
stone parity: raw difference in the number of stones between the player and the opponent
stability score: score based off of the number of "stable" (un-switchable) stones
mobility score: score based off of the number of legal moves available

(2) The most important change I made was making the heuristic dependent on the number of empty squares. The board 
needs to be considered differently depending on the stage of the game. In the extreme late game, only stone parity 
(the raw number of stones for each side) matters, while at the early stage stone parity barely matters. My final 
heuristic function prioritizes positional score in the early game and stone parity in the late game. In the midgame 
there's a mix of positional score, stability score, and mobility score. This outperforms any static heuristic 
function that I have made.

(3) I decided that there is no reason for me to implement iterative deepening, or to go beyond 6 depth. My AI will 
be limited by its heuristic. Without a reliable heuristic, it is useless to look far in the future. In fact, at the end
this is just a contest of heuristics.


Sources:
http://www.samsoft.org.uk/reversi/strategy.htm#evaporate [Value map comes from here.]

http://www.soongsky.com/en/strategy2/ch3.php

http://www.cs.cornell.edu/~yuli/othello/othello.html






Here are a list of heuristics I tried. They are all linear combinations of the "basic" heuristics (like stone 
difference, positional score, mobility score, etc.). I tested them all as black, supposedly the disadvantaged side, 
vs. the bots.

TIME-DEPENDENT NAIVE MIXTURE:
IF more than 35 empty squares
	1 positional score
ELSE IF more than 20 empty squares
	1 positional score
	40 stability score
	5 mobility score
ELSE
	1 stone parity
	
Results (fixed depth 4): win vs. BetterPlayer
Results (fixed depth 6): ~80% vs. ConstantTimePlayer, win vs. BetterPlayer
Results (fixed depth 8): win vs. BetterPlayer


NAIVE MIXTURE
1 adjusted stone parity
1 adjusted corner score
1 adjusted stability score

Results (fixed depth 6): ~80% vs. ConstantTimePlayer, loss vs. BetterPlayer
This isn't a bad result for a completely unscaled 1:1:1 mixture. It's notable, however, that calculating stablity
is extremely expensive.


CORNELL STUDENTS HEURISTIC
IF more than 14 empty squares
	1 stone parity
	100 mobility score
	1000 corner score
ELSE
	1 stone parity

Results (fixed depth 6): ~95% (19/20) vs SimplePlayer, ~50% vs. ConstantTimePlayer, win vs. BetterPlayer
Results (fixed depth 8): loss vs. BetterPlayer
It is highly probable that the 6-depth version's win vs. BetterPlayer was merely luck. BetterPlayer seems to play 
without randomness, so there's no way to test more for that version. However, it is notable that the 8-depth version
loss vs. BetterPlayer. That indicates that the heuristic is poor.
