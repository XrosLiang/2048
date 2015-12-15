# 2048 AI #
-----------------------

Artificial Intelligence for 2048 game. There are 2 applications:

* **console application** - for running multiple games
* **web application** - for observing how the AI works on the [2048 game site](http://gabrielecirulli.github.io/2048/)

## Requirements ##

In order to use the AI you will need to build main application. The requirements are listed below:

* C++11 compiler
* CMake 3.0+
* Boost 1.49.0+ (program_options, accumulators)

If you want to use the web application you will need:

* Python 2
* Chrome or Firefox

Tested on:

* Max OS X 10.11.1, Apple LLVM version 7.0.2
* Ubuntu 15.04 64-bit, g++ 4.9.2
* Windows 7 64-bit, Visual Studio 14 2015

## Strategy files ##

In order to run the application you need to have a strategy file. Some basic (not very good, but small) strategies are already in ```data/2048_strategies/``` directory. We highly encourage you to download an [advanced strategy](http://www.cs.put.poznan.pl/wjaskowski/tmp/tiles-test2-v1.bin.special).

Some strategy files are compressed - that's our way to save some memory. You can either use compressed version or decompress them and speed up calculations.

You can specify the strategy file and unzip option using command line arguments (see Program parameters section).

## Program parameters ##

#### Console application ####

+ **--strategy arg** - strategy input file (by default data/2048_strategies/2048_a_weak_player.bin.txt)
+ **--unzip** - unzip compressed strategy file
+ **--seed arg** - random seed (by default based on time elapsed since epoch)
+ **--games arg** - number of games (by default 1)
+ **--time arg** - maximum time for one round [ms], 0 means no time limit (by default 0)
+ **--depth arg** - maximum depth for expectimax (by default 1)
+ **--threads** - enable multithreading
+ **-h [ --help ]** - produce help message
+ **-v [ --verbose ]** - show board and score after each round

#### Web Application ####

+ **-b [ --browser ]** - choose browser (Chrome or Firefox, by default Firefox)
+ **-p [ --port PORT ]** - port number to control on (default: 32000 for Firefox, 9222 for Chrome)
+ **--strategy arg** - strategy input file (by default data/2048_strategies/2048_a_weak_player.bin.txt)
+ **--unzip arg** - unzip strategy input file (true/false, by default false)
+ **--time arg** - maximum time for one round [ms], 0 means no time limit (by default 0)
+ **--depth arg** - maximum depth for expectimax (by default 1)
+ **--threads arg** - enable multithreading (true/false, by default true)

## Building and running ##

#### Running console application ####

##### Linux/OS X #####

1. Go to the project root directory
2. Run ```cmake .```
3. Run ```make```. The ```lib/``` and ```bin/``` directories will be created.

##### Windows #####

1. Go to the project root directory
2. Create and go to ```_build/``` directory
3. Run ```cmake .. -G "Visual Studio 14 2015 Win64"```. You may have to set boost directories: ```set BOOST_ROOT=your_boost_root_directory``` and ```set BOOST_LIBRARYDIR=your_boost_library_directory``` first
4. Open the solution and build the release version of it

##### Examples #####

* 1 game, max depth 1, single thread, no time limit:
```bash
./bin/main
```
* 1000 games, max depth 1, single thread, no time limit:
```bash
./bin/main --strategy data/2048_strategies/tiles-test2-v1.bin.special --games 1000 --depth 1
```
* 100 games, max depth 3, multiple threads, no time limit:
```bash
./bin/main --strategy data/2048_strategies/tiles-test2-v1.bin.special --games 100 --depth 3 --threads
```
* 10 games, min depth 1, max depth 8, multiple threads, max time 10 ms per round:
```bash
./bin/main --strategy data/2048_strategies/tiles-test2-v1.bin.special --games 8 --depth 8 --time 10 --threads
```

#### Running web application ####

You may need to install the *websocket-client* first:
```
pip install websocket-client
```

##### Chrome #####

1. Close all instances of Chrome (including hangouts, etc...)
2. Run the browser with remote-debugging enabled:
    ```
    chrome --remote-debugging-port=9222
    ```
3. Go to [2048 game site](http://gabrielecirulli.github.io/2048/)
4. Run python script (see examples)

##### Firefox #####

1. First you need to install [Remote Control](https://addons.mozilla.org/pl/firefox/addon/remote-control/) plugin
2. Go to [2048 game site](http://gabrielecirulli.github.io/2048/)
3. Run python script (see examples)

Note that web api is strongly based on https://github.com/nneonneo/2048-ai.

##### Examples #####

* chrome, max depth 1, single thread, no time limit
```
python 2048.py -b chrome
```

* chrome, max depth 4, multiple threads, max time 100 ms per move
```
python 2048.py -b chrome --depth 4 --threads true --time 100
```

On Windows you will need to specify WebApi library file:
```
python 2048.py -b chrome --lib _build/lib/Release/WebApi.dll
```