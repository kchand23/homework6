# Homework 6 - Kentucky Derby
## A Concurrent Program Using PThreads
## Due May 4

The Kentucky Derby is the first of three races in Thoroughbred Racing's "Triple Crown." The 145th Kentucky Derby will run 
May 4th. On that day 20 horses will run 1.25 miles for the chance to split the roughly $2MM Purse!

For this assignment we will simulate a running of the Kentucky Derby using concurrent processes and the pthreads library. 

In our race 20 horses will compete on a track that is 20 lanes wide and 1250 units long. Horses start the race in the lane
with their designated number. For example Horse 0 starts in position (0,0) and Horse 19 starts in position (19, 0). 

When you watch the derby, you'll see that horses start behind a barrier called a "starting gate" which opens at the 
start of the race. We're asking that your horses are synchronized to all start at the same time. Your parent thread 
should print `3, 2, 1 .. GO!` and then the horses can start. 

Our horses can make three moves, which they decide at random. 
* Forward: 2 units towards the finish. (2, 1) -> (2, 3)
* Left: 1 unit towards the inside track and 1 unit toward the finish (2, 1) -> (1, 2)
* Right: 1 unit towards the outside track and 1 unit toward the finish (2, 1) -> (3, 2)

If a horse is on the left or right rail and chooses to move that direction the horse will advance by 1 in the same lane. 
(0, 15) -> (0, 16) or (19, 10) -> (19, 11)

Of course, no two horses can occupy the same position on the track. One could say track positions are MUTually EXclusive. 
A horse trying to move to an occupied position will block until the position is free. The horse should not print its new 
position to stdout until it is in its new position. 

We consider that the jockeys are savvy enough that there is no interference during a move. For instance, a horse 
moving from (1, 1) -> (1, 3) does not collide with a horse moving from (2, 1) -> (1, 2). 

Our horses are also precision machines. Every time they make a move they need to nanosleep for a tenth of a second before making the next. (If we don't yield the thread then our results aren't very exciting.)

Your program execution will be formatted exactly like this (although the horses will be in a different order): 
```BASH
systems1:~> ./a.out
3, 2, 1 .. GO!
Horse 16 starts
Horse 6 starts
Horse 13 starts
Horse 8 starts
Horse 5 starts
Horse 4 starts
Horse 17 starts
Horse 12 starts
Horse 7 starts
Horse 15 starts
Horse 3 starts
Horse 20 starts
Horse 1 starts
Horse 9 starts
Horse 2 starts
Horse 18 starts
Horse 14 starts
Horse 19 starts
Horse 11 starts
Horse 10 starts
Horse: 6, Lane: 14, Position: 2, Lap: 0
Horse: 3, Lane: 16, Position: 1, Lap: 0
Horse: 19, Lane: 1, Position: 2, Lap: 0
Horse: 2, Lane: 19, Position: 1, Lap: 0
...
Horse: 19, Lane: 15, Position: 1248, Lap: 0
Horse: 6, Lane: 11, Position: 1245, Lap: 0
Horse: 19, Lane: 16, Position: 1249, Lap: 0
Horse: 6, Lane: 11, Position: 1247, Lap: 0
Horse: 19, Lane: 16, Position: 0, Lap: 1
Horse: 6, Lane: 12, Position: 1248, Lap: 0
Horse: 6, Lane: 12, Position: 0, Lap: 1
The race finishes!
```

Our autograder will check that no two horses occupied the same space by examining this output, so your formatting must 
match ours exactly. 

Part of what makes this a race is that the threads will start in a non-deterministic order. The sequence of events will 
be different every time you run the program. Even the the starting position of the horses. 

## Project Requirements
* You must use the pthreads.h library. Each horse will be its own thread. 
* You must synchronize your threads so that no horses (threads) start before all other threads have been created and 
the parent has printed `3, 2, 1 .. GO!`.
* No horses can occupy the same position on the track at once. 
* All horses must make it to the finish line and exit the race gracefully. In other words their threads must complete and 
not be forced to join. 
* Of course your program should be free of deadlocks and race conditions. 
* Name your file derby.c and submit to gradescope. 

## Notes on the pthreads library
To compile with the pthreads library you must explicitly link it. This means your program must include the line: 
```C
#include <pthread.h>
```
and be compiled with
```BASH
gcc hw6.c -lpthread
```

## Notes on pthread barriers
This may be a helpful tool, but it is not required. If you wish to use the pthreads barriers you should also 
include the following at the top of your file. 

```C
#define _XOPEN_SOURCE 600 // Required to use the barriers
```

Mac Folks - Sorry but the barriers aren't implemented on macOS. 
