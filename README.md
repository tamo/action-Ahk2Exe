# action-Ahk2Exe

Converts [AutoHotkey](https://github.com/AutoHotkey/AutoHotkey) scripts to executables using [Ahk2Exe](https://github.com/AutoHotkey/Ahk2Exe).

It downloads the latest versions of AutoHotkey and Ahk2Exe.

## inputs

All optional.

- `src`
  - default: "" (just install ahk2exe and base)
  - this is used as the `/in` option
- `taghead`
  - default: `v2.`
  - you can use `v1.1.` for v1 scripts
- `base`
  - default: `_ahktmp/AutoHotkey32.exe`
  - you can omit tmpdir for exe files
    - v2 has `AutoHotkey32.exe` and `AutoHotkey64.exe`
    - v1 has `AutoHotkeyU32.exe`, `AutoHotkeyU64.exe` and `AutoHotkeyA32.exe`
- [`opt`](https://www.autohotkey.com/docs/v2/Scripts.htm#param_pairs)
  - default: ""
  - useful examples are `/out foo.exe` and `/icon foo.ico`
- `tmpdir`
  - default: `_ahktmp`
- `ahk2exe`
  - default: `_ahktmp/Ahk2Exe.exe`

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

#### Or use it multiple times after preparation.

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
        src: RestoreWinPos.ahk

    - run: ls RestoreWinPos.exe

    - name: Ahk2Exe (64bit)
      uses: tamo/action-Ahk2Exe@main
      with:
        src: RestoreWinPos.ahk
        base: AutoHotkey64.exe
        opt: /out RestoreWinPos64.exe

    - run: ls RestoreWinPos64.exe
```
