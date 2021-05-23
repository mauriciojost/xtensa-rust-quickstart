If `rustc` is installed directly with `rustup` there will be no `xtensa` target available:

```
mjost@lapin:~/persospace/xtensa-rust-quickstart (mytest) $ bash flash-esp8266
error: cargo metadata invocation failed: Error during execution of `cargo metadata`: error: failed to run `rustc` to learn about target-specific information

Caused by:
  process didn't exit successfully: `rustc - --crate-name ___ --print=file-names -C link-arg=-Wl,-Tlink.x -C link-arg=-nostartfiles --target xtensa-esp32-none-elf --crate-type bin --crate-type rlib --crate-type dylib --crate-type cdylib --crate-type staticlib --crate-type proc-macro --print=sysroot --print=cfg` (exit code: 1)
  --- stderr
  error: Error loading target specification: Could not find specification for target "xtensa-esp32-none-elf". Use `--print target-list` for a list of built-in targets
```

However if installed a la `xtensa-rust-quickstart` (`rust-xtensa`) all worked fine.
Reminder of instructions (from this project's README.md, to check if up-to-date):

```
$ sudo apt-get install cargo
$ cargo install cargo-xbuild
$ cargo install cargo-espflash
$ git clone https://github.com/MabezDev/rust-xtensa
$ cd rust-xtensa
$ ./configure --experimental-targets=Xtensa
$ ./x.py build --stage 2

and then use xtensa-rust-quickstart/setenv to set some environment varialbes like XARGO_RUST_SRC RUSTC so that the new project builds using the above compiled rust (containing the xtensa target)

```

The builds above in `rust-xtensa` create several binaries to be used, they are under:

```
rust-xtensa/build/x86_64-unknown-linux-gnu/stage0/bin/{cargo,rustc,rustdoc,rust-gdb,rust-gdbgui,rust-lldb}
#and
rust-xtensa/build/x86_64-unknown-linux-gnu/stage2/bin/{rustc,rustdoc}
```
. 
We need the `xtensa` target provided by `stage2/bin/rustc` (the target is named `x86_64-unknown-linux-gnu`), so we need to use the rustc provided after this build (and not the one provided by rustup).


