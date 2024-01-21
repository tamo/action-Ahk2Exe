# action-Ahk2Exe

Converts AutoHotkey v2 scripts to executables with Ahk2Exe.

It downloads the latest versions of AutoHotkey and Ahk2Exe.

(v1 scripts are not supported)

## inputs

Nothing required.

- tmpdir
  - default: _ahktmp
- ahk2exe
  - default: _ahktmp/Ahk2Exe.exe
- base
  - default: _ahktmp/AutoHotkey32.exe
- src
  - default: "" (just install ahk2exe and base)
- opt
  - default: ""

## examples

Simply use it with src.

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

Or prepare and use it multiple times.

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
        base: _ahktmp/AutoHotkey64.exe
        opt: /out RestoreWinPos64.exe

    - run: ls RestoreWinPos64.exe
```
