---
title: "TI-Basic Calculator Programming: Minesweeper and More"
date: 2020-04-15T14:16:42-04:00
draft: false
tags: ["Minesweeper", "TI-Basic"]
category: ["Project"]
---

## Intro

Hello There. I'm back home for corona and found some fun programs on my calculator I wrote in high school. First up: calculator minesweeper.

## Features


This version of minesweeper can't quite compete with what you're used to, but it has some handy functionality that gets you thinking about Minesweeper at a lower level. You'll also get to see what TI-Basic looks like!

 - Saving games
 - Clearing adjacent mines when revealing a zero
 - Starting the game on a zero
 - Keeping track of time


## User Experience

When starting up the program, you will have the option to play on a new board, to replay the last board, or to load a saved game. The program stores previous games in calculator matrix variables which work nicely for grid structured games like this one.

{{< figure src="menu_screen.png" >}}

<details style="margin-bottom: 2.5em" open>
    <summary>Get a glimpse of TI-Basic code</summary>
    {{< highlight python >}}
Menu("MINE SWEEPER","QUICK START",0,"NEW GAME",1,"LOAD GAME",2)

" quick start
" prgmPro manages the interactive portion of the game
" you'll realize that I was quite horrible at naming variables/programs back then
Lbl 0
prgmPRO
Goto 9

" generate a new game
" prgmDONE generates the new board
Lbl 1
prgmDONE
prgmPRO
Goto 9

" prgmSAVE loads the previously saved game
Lbl 2
prgmSAVE
Goto 9
{{< /highlight >}}
</details>


The replay last board option was mainly intended to make the long board generation times bearable. These calculators are not especially fast; it takes about 30 seconds. When loading a new game, you can see the controls. Arrows to move, 8 to toggle the flag, and enter to reveal. 

{{< figure src="loading_screen.png" >}}
<details style="margin-bottom: 2.5em" open>
    <summary>Code for calculating Percent Loaded</summary>
    {{< highlight python >}}
" U represents percent loaded (all variables are single characters)
" correctly position based on number of digits in U
If U<10
Then
Output(1,18,U)
Else
Output(1,17,U)
End

" calculate neighboring mines
" [D] is the matrix that stores the board
" values of -1 correspond to mines
" 0 is a temp variable for storing adjacent mines
0→θ
If [D](I,J)≠⁻1
Then
" special case when on a wall
If I=1 or I=10 or J=1 or J=26
Then
" loop over adjacent squares
For(S,⁻1,1,1)
For(T,⁻1,1,1)
If I+S>0 and I+S<11 and J+T>0 and J+T<27
Then
If [D](I+S,J+T)=⁻1
θ+1→θ
End
End
End
Else
" case when not on wall
For(S,⁻1,1,1)
For(T,⁻1,1,1)
If [D](I+S,J+T)=⁻1
θ+1→θ
End
End
End
"save mine counts to E
θ→[E](I,J)
End
End
End
{{< /highlight >}}
</details>




Once finished loading the board, you start the game with your cursor over a randomly selected 0. I partially implemented the functionality that clears the surrounding when revealing a zero, however it doesn't fully capture the zero lake as implementing such functionality would make the program run even slower than it already does.

{{< figure src="clearMultiple.png" >}}
<details style="margin-bottom: 2.5em" open>
    <summary>Code for recursively revealing zero lake</summary>
    {{< highlight python >}}
" reveal surrounding squares when nearby mines = 0
For(G,⁻1,1,1)
For(H,⁻1,1,1)
If Y+G>0 and Y+G≤10 and X+H>0 and X+H≤26
Then
1→[A](Y+G,X+H)
Output(Y+G,X+H,[E](Y+G,X+H)
" if it's zero again, clear again
If [E](Y+G,X+H)=0
Then
" variables are global 
" in order to save these values I had to store them in temp variables
Y→𝗡
X→I%
G→PV
H→PMT
X+H→X
Y+G→Y
prgmCLEAR2
𝗡→Y
I%→X
PV→G
PMT→H
End

End
End
End
{{< /highlight >}}
</details>


You can mark potential mine locations with a "P" flag. If you reveal a mine, all the mines are revealed before wiping the screen away. When the game ends, you are presented with your outcome, the time you played for, and the number of mines. I'm pretty sure it took me longer to code this than I spent playing it.

{{< figure src="endgame.png" >}}
<details style="margin-bottom: 2.5em" open>
    <summary>Code for revealing mines and clearing screen</summary>
    {{< highlight python >}}
" reveal all the mines
0→K
0→J
" the board is 10x26
For(N,1,10,1)
For(O,1,26,1)
If [D](N,O)=⁻1
Then
1+J→J
Output(N,O,"X")
End
End
End
Pause 
" replace board with blank
1→W
For(N,1,10,1)
For(O,1,26,1)
Output(N,O," ")
If [D](N,O)=0
Then
0→W
0→E
End
End
End
{{< /highlight >}}
</details>



Looking back, I'm surprised I was able to persevere through writing out the program. No vim bindings, no indentation, and the screen holds just 9 lines of code! Upon opening the code back up it took me a while to reorient myself (there were zero comments at the time) as the syntax is just a small step above assembly. I think most of the time I spent putting this together was during math class.

{{< comment >}}
One section of the code that stuck out to me was the following (captured from CEmu emulator): 

{{<figure src="code_snip.png" >}}

After learning assembly, this reminds me of saving variables on the stack when calling a subroutine, and then recovering them afterwards. But the calculator equivalent (I had to dig pretty deep to find these weird variables).
{{< /comment >}}

## Bonus

### Prank 

The fun doesn't end there. I remember some of my favorite programs to play around with were "Prank" programs which run their mission in "stealth" mode without the user noticing. One such program appears to parse arithmetic operations normally, but occasionally adjusts the output of an arithmetic expression by minor amounts, potentially leading someone to think their calculator is broken.

If you are very fast you can code and run this program on a friends calculator while they aren't paying attention. :)

{{<figure src="prank2.png" >}}

### Guessing Game

Simple guessing game for a number between 1 and 10000 with a first place leader board. This is the first time I became familiar with binary search.

{{<figure src="guess.png" >}}

### PE

Seems to correspond to some math equation, but I haven't been able to figure out which one. Let me know if you figure it out!

{{<figure src="pe.png" >}}

### Flappy Bird

Before Minesweeper I put together a Flappy Bird "clone", but unfortunately it doesn't seem to have stood the test of time. I vaguely recall wincing as someone reset the RAM of my calculator along with all the code.

## Resources

You can find the code for the above and more on [GitHub](https://github.com/Ruborcalor/ti84-minesweeper) if you'd like to give it a whirl on your very own ti device / emulator. Thanks for tuning in!
