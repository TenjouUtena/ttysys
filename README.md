ttysys v0.8
======

## About
ttysys is a live ncurses cpu usage graph.
The graph's units are in percentage.
It updates once every second.

### Screenshot
<a href="https://raw.github.com/rigel314/ttysys/26abbe0964db8f627d064974d4958542c10edd65/images/ttysys.png">![ttysys example](https://raw.github.com/rigel314/ttysys/26abbe0964db8f627d064974d4958542c10edd65/images/ttysysSmall.png)</a>

## Build
    $ sudo apt install libncurses5-dev
    $ make all

Build takes a dependency on `libncurses5-dev`. Simply `sudo apt install libncurses5-dev` for Debian/Ubuntu.
A simple `make all` should suffice.  If you get errors using my makefile, see the Questions/Bugs section below.

## Installation
    $ make install
Running `make install` will copy the binary to `/usr/local/bin`.  This should be in your path.

### Uninstallation
    $ make uninstall
Running `make uninstall` will `rm /usr/local/bin/ttysys`.

## Usage
    $ ttysys [<sequence>]
_sequence_ can consist of any number of the following characters:

* `?` - Displays a help window.
* `h` - Split current window horizontally.
* `v` - Split current window vertically.
* `c` - Close current window.
* Tab - Move to next window in order of creation.
* Arrow Keys - Move to next window on screen in direction pressed.
* Numbers `0` - `9` and keys `m` and `s` - Select a data source for a window.
	* `0` will set the data source to a CPU overview, and `1` - `9` set it to a specific core.
        * `Shift` `0` - `9` Will set to CPUs 10-19
	* `m` will set the data source to RAM usage.
	* `s` will set the data source to Swap usage.
* `g` - Toggle grid for selected window.
* `e` - Toggle value display in current window's title.
* `t` - Toggle display of current window's title bar.
* `o` - Toggle display of current window's ordinate label sidebar.
* `q` - Quit this program.
* `u` - Same as up arrow key. Move to window above current one.
* `d` - Same as down arrow key. Move to window below current one.
* `l` - Same as left arrow key. Move to window left of current one.
* `r` - Same as right arrow key. Move to window right of current one.

These same buttons will control the program while it is running.

## How it works
### CPU
It works by reading the first few lines in `/proc/stat` that begin with cpu.<br />
`man 5 proc` explains the meaning of the contents of `/proc/stat`.<br />
These lines tell you how much time each CPU spent in different states.  The sum of each line is the total time spent for each CPU.  I read this file twice with a second in between.  Then, I subtract the two totals to have the total CPU time spent during my `sleep()`.  Now, I add the user and system numbers together and divide by my difference.  Finally, it's just a matter of displaying it nicely.

### Memory
To check the RAM and Swap, I read the appropriate lines in `/proc/meminfo`.  MemTotal, MemFree, Buffers, Cached, SwapTotal, and SwapFree are all the lines that ttysys reads.  MemFree + Buffers + Cached is the amount of free RAM usually reported by other tools, so I conformed.

## Contributions
I will be sticking to the branching model described [here](http://nvie.com/posts/a-successful-git-branching-model/).  Pull requests should also stick to this branching model.  Thanks.

## Questions/Bugs?
[Report A Bug](https://github.com/rigel314/ttysys/issues)<br />
OR<br />
Contact me at <pi.rubiks@gmail.com>
