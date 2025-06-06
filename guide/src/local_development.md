# Local Development

## `maturin develop` command

For local development, the `maturin develop` command can be used to quickly
build a package in debug mode by default and install it to virtualenv.

```
Usage: maturin develop [OPTIONS] [ARGS]...

Arguments:
  [ARGS]...
          Rustc flags

Options:
  -b, --bindings <BINDINGS>
          Which kind of bindings to use

          [possible values: pyo3, pyo3-ffi, cffi, uniffi, bin]

      --strip
          Strip the library for minimum file size

  -E, --extras <EXTRAS>
          Install extra requires aka. optional dependencies

          Use as `--extras=extra1,extra2`

      --skip-install
          Skip installation, only build the extension module inplace

          Only works with mixed Rust/Python project layout

      --pip-path <PIP_PATH>
          Use a specific pip installation instead of the default one.

          This can be used to supply the path to a pip executable when the current virtualenv does not provide one.

  -q, --quiet
          Do not print cargo log messages

      --ignore-rust-version
          Ignore `rust-version` specification in packages

  -v, --verbose...
          Use verbose output (-vv very verbose/build.rs output)

      --color <WHEN>
          Coloring: auto, always, never

      --config <KEY=VALUE>
          Override a configuration value (unstable)

  -Z <FLAG>
          Unstable (nightly-only) flags to Cargo, see 'cargo -Z help' for details

      --future-incompat-report
          Outputs a future incompatibility report at the end of the build (unstable)

      --uv
          Use `uv` to install packages instead of `pip`

      --compression-method <COMPRESSION_METHOD>
          Zip compresson method to use

          [default: deflated]

          Possible values:
          - deflated: Deflate compression
          - stored:   No compression
          - zstd:     Zstandard compression

  -h, --help
          Print help (see a summary with '-h')

Compilation Options:
  -r, --release
          Pass --release to cargo

  -j, --jobs <N>
          Number of parallel jobs, defaults to # of CPUs

      --profile <PROFILE-NAME>
          Build artifacts with the specified Cargo profile

      --target <TRIPLE>
          Build for the target triple

          [env: CARGO_BUILD_TARGET=]

      --target-dir <DIRECTORY>
          Directory for all generated artifacts

      --timings=<FMTS>
          Timing output formats (unstable) (comma separated): html, json

Feature Selection:
  -F, --features <FEATURES>
          Space or comma separated list of features to activate

      --all-features
          Activate all available features

      --no-default-features
          Do not activate the `default` feature

Manifest Options:
  -m, --manifest-path <PATH>
          Path to Cargo.toml

      --frozen
          Require Cargo.lock and cache are up to date

      --locked
          Require Cargo.lock is up to date

      --offline
          Run without accessing the network
```

## PEP 660 Editable Installs

Maturin supports [PEP 660](https://www.python.org/dev/peps/pep-0660/) editable installs since v0.12.0.
You need to add `maturin` to `build-system` section of `pyproject.toml` to use it:

```toml
[build-system]
requires = ["maturin>=1.0,<2.0"]
build-backend = "maturin"
```

Editable installs can be used with mixed Rust/Python projects so you
don't have to recompile and reinstall when only Python source code changes.
They can also be used with mixed and pure projects together with the
[import hook](./import_hook.md) so that recompilation/re-installation
occurs automatically when Python or Rust source code changes.

To install a package in editable mode with pip:

```bash
cd my-project
pip install -e .
```

or

```bash
cd my-project
maturin develop
```

Then Python source code changes will take effect immediately because the interpreter looks
for the modules directly in the project source tree.
