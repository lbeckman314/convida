# Convida

<img src="https://github.com/user-attachments/assets/10207ae4-2a1a-441d-b29e-b9fdec360e84" width=250/>

An implementation of [Conway's Game of Life](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life) based on this [tutorial](https://rustwasm.github.io/docs/book/game-of-life/introduction.html).

## Quick Start

To start the server, enter:

```
git clone https://github.com/lbeckman314/convida
cd convida/www
npm install
npm run start
```

[![Screenshot of Convida](https://github.com/user-attachments/assets/d43383ed-24d5-4a25-89b9-6f8e20f320fa)](https://convida.liambeckman.com)


## Development

### Dependencies

wasm-pack
- [Quick start method](https://rustwasm.github.io/wasm-pack/installer/#)
- Cargo method: `cargo install wasm-pack`

rust
- [Quick start method](https://www.rust-lang.org/learn/get-started)

Then, within the convida directory, enter:

```sh
wasm-pack build
```

to build the Rust code in `src/lib.rs` to the `pkg` directory.

### Source Descriptions

| File/Directory                         | Description                                      |
|----------------------------------------|--------------------------------------------------|
| [Cargo.lock](./Cargo.lock)             | dependencies.                                    |
| [Cargo.toml](./Cargo.toml)             | dependencies.                                    |
| [README.md](./README.md)               | this README file.                                |
| [benches](./benches)                   | used in optimizing runtime.                      |
| [convida-alt.png](./convida-alt.png)   | alternative logo image.                          |
| [convida.png](./convida.png)           | logo image.                                      |
| [convida.xcf](./convida.xcf)           | GIMP logo file.                                  |
| [cross-compile.sh](./cross-compile.sh) | beta cross compiling script.                     |
| [perf.data](./perf.data)               | used in optimizing runtime.                      |
| [pkg](./pkg)                           | destination directory for compiled rust code.    |
| [screenshot.png](./screenshot.png)     | screenshot image.                                |
| [src](./src)                           | main source files.                               |
| [target](./target)                     | Rust's target directory.                         |
| [tests](./tests)                       | used in testing ticks and created cell patterns. |
| [www](./www)                           | directory for web client.                        |


## Alternatives

- http://golly.sourceforge.net/
- https://copy.sh/life/

## Improvements on the Tutorial

- Introduce an `<input type="range">` widget to control how many ticks occur per animation frame.
- Add a button that resets the universe to a random initial state when clicked. Another button that resets the universe to all dead cells.
- On Ctrl + Click, insert a glider centered on the target cell. On Shift + Click, insert a pulsar.
- At this point, the next lowest hanging fruit for speeding up Universe::tick is removing the allocation and free. Implement double buffering of cells, where the Universe maintains two vectors, never frees either of them, and never allocates new buffers in tick.

## TODO

- [ ] Implement the alternative, delta-based design from the "Implementing Life" chapter, where the Rust code returns a list of cells that changed states to JavaScript. Does this make rendering to `<canvas>` faster? Can you implement this design without allocating a new list of deltas on every tick?
- [ ] As our profiling has shown us, 2D `<canvas>` rendering is not particularly fast. Replace the 2D canvas renderer with a WebGL renderer. How much faster is the WebGL version? How large can you make the universe before WebGL rendering is a bottleneck?
- [ ] We only ever instantiate a single Universe, so rather than providing a constructor, we can export operations that manipulate a single static mut global instance. If this global instance also uses the double buffering technique discussed in earlier chapters, we can make those buffers also be static mut globals. This removes all dynamic allocation from our Game of Life implementation, and we can make it a `#![no_std]` crate that doesn't include an allocator. How much size was removed from the .wasm by completely removing the allocator dependency?
