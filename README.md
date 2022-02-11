# ECP-VELOC GitHub Actions

These actions are for use by the ECP-VELOC components, namely:

- [kvtree](https://github.com/ecp-veloc/kvtree)
- [spath](https://github.com/ecp-veloc/spath)
- [rankstr](https://github.com/ecp-veloc/rankstr)
- [axl](https://github.com/ecp-veloc/axl)
- [redset](https://github.com/ecp-veloc/redset)
- [shuffiles](https://github.com/ecp-veloc/shuffiles)
- [er](https://github.com/ecp-veloc/er)

## Actions

### `get-scr-os-deps`

Install the OS packages, including:
- MPI (if called for, on by default)
- pdsh
- lcov (for code coverage)

This action works for both ubuntu and macOS targets.

### `build-ecp-veloc-component`

This action will:

1. checkout the named component repo, with the option of checking out a particular SHA or tag ref.
2. Build the component using CMake, with the option of building the static version of the library
3. Install the built component

### `cmake-build-static`

Build the static only version of the specified component

### `cmake-build-only`

Build the specified component, with default cmake options
*Note:* Do no use in conjunction with `cmake-build-test`.

### `cmake-build-test`

Build the specified component, with default cmake options.
Then run the cmake tests with `make check`.
*Note:* Do no use in conjunction with `cmake-build-only`.
