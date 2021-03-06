# Tic-Tac-Toe

### Getting Started

#### What is it?
A simple, elegant, Tic-Tac-Toe game to be played via a command line, optimised for use in a
UNIX terminal.

#### Who is it for?
Anyone who enjoys playing Tic-Tac-Toe! The game supports either one or two players, depending on
the selected game mode.

#### Ace! How can I play it?
Good question. Just follow these simple steps:
1. Clone [this](https://github.com/michaelbjacobson/tic-tac-toe.git) GitHub repo onto your machine
2. Using your terminal, navigate into the main Tic-Tac-Toe directory eg. `$ cd ~/tic-tac-toe`
3. Once inside Tic-Tac-Toe's main directory run the `$ bundle` command to gather the necessary dependencies
4. Next, navigate into Tic-Tac-Toe's lib directory by entering the following command `$ cd lib`
5. Finally, simply enter `$ ruby game.rb` and away you go!
6. Enjoy the game! If, for whatever reason, you'd like to finish the game early, simply type 'ctrl + c'
into your terminal.

#### Great, is there anything else I should know?
Yes. The computer player is __unbeatable__. I'm not kidding, it literally can't be beaten. If you
play against the computer you'll either draw or lose. If you don't believe me, give it a try. I dare you.

Also, you may have noticed a file called 'config', inside the main Tic-Tac-Toe directory. This file allows you to
quickly and easily make certain changes, before runtime, to the way the program operates. Feel free to experiment with
different settings, though do bear in mind that if either of the test options aren't set to 'enabled' or 'disabled',
they will be overridden by default settings. Ditto if the computer options are set below 0. 

It's interesting to see at what level of move evaluation the computer player stops being infallible!

#

### Testing

#### How do I run the tests?
1. First things first, go ahead and clone the repo onto your local machine and bundle the dependencies (steps 1 
through 3 in the _'How can I play it?'_ sub-section of the _'Getting Started'_ section, above).
2. Next, ensure that you are in the main Tic-Tac-Toe directory. If in doubt, you can easily check which directory you're in by
entering the following in your terminal: `$ pwd`.
3. Enter the `$ rspec` command into your terminal.
4. Watch all the tests pass!

#### Anything else I should know about these tests?
Yup. There are 47 integration tests, some of which are *very* slow. These tests are enabled by default but can easily
be disabled thus:
1. Navigate into the main Tic-Tac-Toe directory, if you aren't already there.
2. Open the 'config' file, contained therein, using your favourite text editor, eg. `$ vim config`.
3. Find the line that says 'integration_tests' and change 'enabled' to 'disabled'.
4. Save the change, and close the file.
5. Run the tests! (See _'How do I run the tests?'_ above).

#

### My Process

#### Part One
I began by dissecting and refactoring the original codebase. The very first step of this was tweaking the original
script to make it compliant with the [Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide). Next I
trimmed out a lot of redundant code in several methods. Finally I extracted several classes from the one existing
'God' class, in adherence with the single responsibility principle.

Then I tackled the specific issues highlighted in the problem brief. I decided to begin by ensuring that the computer
player would be unbeatable. I did some research and decided that using the minimax algorithm would be the optimal
approach. After some more research and some spiking I successfully implemented the minimax algorithm into the Computer
class. After this I added additional functions into the Board class to allow the Computer to assess it to be able to
choose its next move. Next I created a Player superclass, from which both the Human and Computer classes inherited.
I made use of duck-typing and allowed the Human and Computer classes to both respond to the #make_move method, as 
called by the Game class. Finally I built the Game class up to wrap the other classes and act as an engine for games.

The final issue that was pointed out in the brief was how poor and inelegant the gameplay was. I was very conscious
that I didn't want to implement redundant functionality that the client may not want or need. However, I felt that
the task of improving the user experience allowed a certain amount of creativity on my part. I am particularly pleased
with the solution of having two parallel grids - one representing the active game board and another the numbered key
for the board tiles. This allows players to see a clear and uncluttered picture of the game while also having a useful
reference if they're uncertain about which number corresponds to which tile. I was also pleased with my decision to 
highlight the winning row in red when the game is won. 

Next I implemented the additional functionality requested in the original brief, that is the ability for the user to
select the game mode, their symbol, and which player should make the first move.

It goes without saying that I test-drove the development of the original refactor, the class extraction and also the
implementation of new functionality. I used the RSpec testing framework, with the Simplecov gem to monitor test
coverage. Some tests written early on were later made redundant and hence removed from the test suite. In total I am
making use of 46 tests, which give total coverage of 96%. I had difficulties unit testing recursion and so opted
to implement a more feature-oriented test structure for some recursion-dependent functionality. I would love to learn
more about effective testing of recursion!

#### Part Two

For the second part of this project, I was tasked with adding the functionality for games to be played on a 4x4 grid
instead of the usual 3x3 one.

Initially simply added the ability for the computer to assess a 4x4 board. I was hardly expected to discover that, 
using my existing algorithm, it took the computer several hours to work out it's first move! I then did a lot of
studying eg. watching lectures online, reading blog posts/articles and also dipping into some books. Based on my
research I decided to try two ways to speed up my computer player: alpha-beta pruning, and transposition tables.

After finding little success with my, albeit very crudely built, transposition table, I decided that alpha-beta pruning
was the way to go. I also discovered that implementing alpha-beta pruning is considerably less fiddly when added to
a negamax, as opposed to a minimax, algorithm. After some extensive spiking, and more reading etc, I managed to get my
computer playing the same perfect games much, _much_ faster by using said negamax algorithm with alpha-beta pruning.
While I did find some interesting articles online I mostly based my new algorithm on the pseudocode found 
[here](https://en.wikipedia.org/wiki/Negamax).

Once I was happy with the performance of my computer player on a 4x4 grid, I set about overhauling my entire test suite,
especially the integration tests and AI behaviour/performance tests.

Based off feedback from players, I changed the user interface somewhat, removing the two grids and using only one. In
order to make it clearer which tiles contained user symbols and which simply their own index, I decided to extend my
colour functionality to all user moves, not just the winning row. I also changed the lowest number on the grid to 1, 
instead of 0, as per other user feedback. Based on several other players's feedback, I decided to clear the display
after the setup and after each move, to make it clearer what change had taken place since the previous turn. 

After squashing a few more miscellaneous bugs, I decided to add a config file to make it simpler for less technical
users to modify the program before runtime. The config file is evaluated when the program or the test suite is run and
environment variables are set according to it. While this wasn't explicitly requested in the brief, I felt that it, like
the coloured symbols, fell within the realm of improving the user experience.

All in all, I'm very pleased with the game in it's current form, but am looking forward to improving it further!

### Click [here](https://www.youtube.com/watch?v=b5L-cKN1mMA&feature=youtu.be) to see a demonstration of the game being installed and played on my Raspberry Pi 3! 
