# ECP-VELOC GitHub Actions

These actions are for use by the ECP-VELOC components, namely:

- [kvtree](https://github.com/ecp-veloc/kvtree)
- [spath](https://github.com/ecp-veloc/spath)
- [rankstr](https://github.com/ecp-veloc/rankstr)
- [axl](https://github.com/ecp-veloc/axl)
- [redset](https://github.com/ecp-veloc/redset)
- [shuffiles](https://github.com/ecp-veloc/shuffiles)
- [er](https://github.com/ecp-veloc/er)

Note that these are "composite actions" not "reusable workflows" in GitHub Actions parlance.
More details on composite actions can be found in the [GitHub Actions docs](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action).

## Usage

Each component uses the actions below with an `@main` version specifier.
This means that if the API for these actions changes, that will temporarily "break" the component actions.
This is expected.
It is more likely that we want each component to be using the latest version of an action than the API for the actions overall changes.

**If YOU break the API for actions, it is expected that YOU will manually fix the actions for each of the components.**

An alternate development path is to:

1. introduce a new action(s)
2. migrate components to the new action(s)
3. remove old actions

## Actions

### `get-scr-os-deps`

Install the OS packages, including:
- MPI (if called for, on by default)
- pdsh
- lcov (for code coverage)
- (optional) autotools, including libtool, automake, and autoconf.
  These are needed for lwgrp and dtcmp, but not the components.

This action works for both ubuntu and macOS targets.

### `build-ecp-veloc-component`

This action will:

1. checkout the named component repo, with the option of checking out a particular SHA or tag ref.
2. Build the component using CMake, with the option of building the static version of the library
3. Install the built component

This action has a "static-only" option.

(This action is quite similar to the `cmake-build` action.
See below for a note on why they are distinct.)

### `cmake-configure`

This configures (runs `cmake PATH`) the current component, it does not build (run `make`).
Options:

| argument | description | required? | default |
|:--|:--|:--|:--|
| `component` | name of the component | required | |
| `target`    | CMake Build Type      | optional | Release |
| `cmake_line`| Additional CMake build options | optional | "" |
| `static`    | Build only the static library  | optional | false |

#### Note

This action is somewhat similar to `build-ecp-veloc-component` and it has many of the same options.
However, this actions is intended to be called for the component we are currently testing.
It is important to separate the "configure", "build", and "test" steps for the component to identify where any failure occurs.
Therefore, it is conceivable that this action may fail, whereas the `build-ecp-veloc-component` action is *never* expected to fail.

### `cmake-build`

Run `make` for the current component.

### `cmake-test`

For components that can be tested, this runs `make check` (a verbose version of `make test`).

It is intended that *all* components will have a `cmake-test` step in their build-and-test action.
However, for components where multi-node testing is necessary, the test step can be disabled with an `if: false` qualifier.


## Deprecated Actions

Once all components are migrated, these actions will be removed.

- cmake-build-only
- cmake-build-static
- cmake-build-test
