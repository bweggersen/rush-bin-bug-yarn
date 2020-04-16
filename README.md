# Repro of issue with missing internal bin scripts in Rush with yarn

Reference: https://github.com/microsoft/rushstack/issues/1100

## Usage

```
git clone
rush update
cd packages/foo/node_modules/bin
ls
```

You can also try a pnpm version for reference (commit: `b687566b84580bf6520abae6df799ff589d6dca8`).

## Tree view of `packages/`

### Using yarn

```
❯ tree -l -a
.
├── bar
│   ├── .rush
│   │   └── temp
│   ├── bin
│   │   └── bar.js
│   └── package.json
└── foo
    ├── .rush
    │   └── temp
    ├── node_modules
    │   ├── .bin -> ../../../common/temp/node_modules/.bin
    │   │   └── prettier -> ../prettier/bin-prettier.js
    │   ├── bar -> ../../bar  [recursive, not followed]
    │   └── prettier -> ../../../common/temp/node_modules/prettier
    │       ├── LICENSE
    │       ├── ...
    │       └── third-party.js
    └── package.json

11 directories, 22 files
```

### Using pnpm

```
❯ tree -l -a
.
├── bar
│   ├── .rush
│   │   └── temp
│   │       └── shrinkwrap-deps.json
│   ├── bin
│   │   └── bar.js
│   └── package.json
└── foo
    ├── .rush
    │   └── temp
    │       └── shrinkwrap-deps.json
    ├── node_modules
    │   ├── .bin
    │   │   ├── bar
    │   │   └── prettier
    │   ├── bar -> ../../bar  [recursive, not followed]
    │   └── prettier -> ../../../common/temp/node_modules/.pnpm/registry.npmjs.org/prettier/2.0.4/node_modules/prettier
    │       ├── LICENSE
    │       ├── ...
    │       └── third-party.js
    └── package.json

11 directories, 25 files
```
