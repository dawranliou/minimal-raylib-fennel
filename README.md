# Minimal Fennel Setup for Raylib

Minimal [Fennel][fennel] setup for [Raylib][raylib] using the
[LuaJIT][luajit]-based binding via [raylib-lua][raylib-lua].

## Prerequisites

1. Compiled binaries from the [raylib-lua][raylib-lua] project available on the
   system's path:
   - `raylua_s` for script-mode programs
   - `raylua_e` for embedding-mode programs

## Usage

Run the example program as a script:

``` shell
$ raylua_s main.lua
```

Compile the example program to an executable binary:

``` shell
$ mkdir -p target
$ cp fennel.lua main.lua main.fnl target
$ raylua_e target
$ ./target_out
```

## Explanation

The [raylib-lua][raylib-lua] project does most of the heavy-lifting to create
Lua programs using the Raylib game library.  Embedding the Fennel compiler into
the Lua program is a thin, extra layer on top of the stack.  With the release of
[Fennel 1.2.1][fennel 1.2.1], you can add the following one-liner to extend
Lua's built-in `require` function to also support Fennel.  (See Fennel's [API
documentation][require fennel] and [this section from the Setup
Guide][embedding-fennel].)

``` lua
-- main.lua
require("fennel").install().dofile("main.fnl")
```

The above line in the `main.lua` file loads the example `main.fnl` code:

``` fennel
;; main.fnl
(rl.SetConfigFlags rl.FLAG_VSYNC_HINT)

(rl.InitWindow 800 450 "raylib [core] example - basic window")

(while (not (rl.WindowShouldClose))
  (rl.BeginDrawing)
  (rl.ClearBackground rl.RAYWHITE)
  (rl.DrawText "Congrats! You created your first window!" 190 200 20 rl.LIGHTGRAY)
  (rl.EndDrawing))

(rl.CloseWindow)
```

[Raylib-lua][raylib-lua] automatically binds the Raylib C APIs to the global
`rl` table.  You can learn the Raylib APIs from the [Raylib cheatsheet][raylib
cheatsheet].

## Next

I'd like to keep this project minimal like `~benthor`'s repo -
[absolutely-minimal-love2d-fennel][absolutely-minimal-love2d-fennel].  So I
almost consider this project done.  However, I do want to start other projects
to explore the REPL support and how much I can push the interactive programming
workflow based on the current setup.

## Credits

- [Raylib][raylib]
- [alexjgriffith/min-love2d-fennel][min-love2d-fennel] - I've learned a lot from
  this project template about Fennel and Love2d.
- [~benthor/absolutely-minimal-love2d-fennel][absolutely-minimal-love2d-fennel]
  for explaining in details the absolute minimal setup for a Fennel Love2d
  project.
- [MattRoelle/minimal-raylib-lua-fennel][minimal-raylib-lua-fennel] for
  inspiring this experimental project.

[lua]: http://www.lua.org/
[raylib]: https://www.raylib.com/
[raylib cheatsheet]: https://www.raylib.com/cheatsheet/cheatsheet.html
[fennel]: https://fennel-lang.org/
[raylib-lua]: https://github.com/TSnake41/raylib-lua
[luajit]: https://luajit.org/
[absolutely-minimal-love2d-fennel]: https://git.sr.ht/~benthor/absolutely-minimal-love2d-fennel
[min-love2d-fennel]: https://gitlab.com/alexjgriffith/min-love2d-fennel
[embedding-fennel]: https://fennel-lang.org/setup#embedding-the-fennel-compiler-in-a-lua-application
[require fennel]: https://fennel-lang.org/api#use-luas-built-in-require-function
[fennel 1.2.1]: https://git.sr.ht/~technomancy/fennel/tree/main/changelog.md#121--2022-10-15
[minimal-raylib-lua-fennel]: https://github.com/MattRoelle/minimal-raylib-lua-fennel
