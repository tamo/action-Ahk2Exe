# action-Ahk2Exe

Converts [AutoHotkey](https://github.com/AutoHotkey/AutoHotkey) scripts to executables using [Ahk2Exe](https://github.com/AutoHotkey/Ahk2Exe).

It downloads the latest versions of AutoHotkey and Ahk2Exe.

## inputs

All optional.

- `src`
  - default: "" (just install base and ahk2exe)
  - if specified, this is used as the `/in` option
    - this can be a comma-separated list such as `a.ahk,b.ahk,c.ahk`
- `taghead`
  - default: `v2.`
  - you can use `v1.1.` for v1 scripts
- `base`
  - default: `_ahktmp/AutoHotkey32.exe`
  - you can omit `tmpdir` for exe files
  - this can be a comma-separated list such as `AutoHotkey32.exe,AutoHotkey64.exe`
    - if this contains multiple files, generated files will have suffix such as `32`, `64`, `U32`, `U64`, or `A32`
    - v2 has `AutoHotkey32.exe` and `AutoHotkey64.exe`
    - v1 has `AutoHotkeyU32.exe`, `AutoHotkeyU64.exe` and [`AutoHotkeyA32.exe`](https://www.autohotkey.com/docs/v1/Compat.htm#Format)
- [`opt`](https://www.autohotkey.com/docs/v2/Scripts.htm#param_pairs)
  - default: ""
  - mostly for `/icon foo.ico`
- `tmpdir`
  - default: `_ahktmp`
  - other values (`base` and `ahk2exe`) don't refer to this value, so if you change this you have to change them as well
- `ahk2exe`
  - default: `_ahktmp/Ahk2Exe.exe`
  - AutoHotkey v1 is shipped with its own compiler, so you can use `_ahktmp/Compiler/Ahk2Exe.exe` if `taghead` points to v1

## Examples

See [ci.yml](https://github.com/tamo/action-Ahk2Exe/blob/main/.github/workflows/ci.yml) in this repo.

#### Simply use it with `src`.

```yaml
jobs:
  build:
    runs-on: windows-latest

    steps:
    - name: Checkout
      uses: actions/checkout@v4

    - name: Ahk2Exe (32bit)
      uses: tamo/action-Ahk2Exe@main
      with:
        src: RestoreWinPos.ahk

    - run: ls RestoreWinPos.exe
```

#### Specify `base` to embed 64bit binary.

```yaml
jobs:
  build:
    runs-on: windows-latest

    steps:
    - name: Checkout
      uses: actions/checkout@v4

    - name: Ahk2Exe (64bit)
      uses: tamo/action-Ahk2Exe@main
      with:
        base: AutoHotkey64.exe
        src: RestoreWinPos.ahk

    - run: ls RestoreWinPos.exe
```

#### Specify multiple targets.

```yaml
jobs:
  build:
    runs-on: windows-latest

    steps:
    - name: Checkout
      uses: actions/checkout@v4

    - name: Prepare Ahk2Exe
      uses: tamo/action-Ahk2Exe@main

    - run: |
        ls _ahktmp/Ahk2Exe
        ls _ahktmp/AutoHotkey32.exe
        ls _ahktmp/AutoHotkey64.exe

    - name: Ahk2Exe (32bit)
      uses: tamo/action-Ahk2Exe@main
      with:
        src: RestoreWinPos.ahk,AnotherSource.ahk

    - run: |
        ls RestoreWinPos.exe
        ls AnotherSource.exe

    - name: Ahk2Exe (multi-target)
      uses: tamo/action-Ahk2Exe@main
      with:
        src: RestoreWinPos.ahk,AnotherSource.ahk
        base: AutoHotkey32.exe,AutoHotkey64.exe

    - run: |
        ls RestoreWinPos32.exe
        ls RestoreWinPos64.exe
        ls AnotherSource32.exe
        ls AnotherSource64.exe
```
