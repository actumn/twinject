# twinject
`This project is a work-in-progress.`

`This has nothing to do with the auto-injector.`

## What is this?
_twinject_ (**t**ouhou **w**indows **inject**or) is a framework that exposes functions and data in Touhou games for modding.

So far, I plan to only implement for *TH08 Imperishable Night* with the dxd9 patch, because I like IN.

## Functionality
Consists of two parts, **twinhook** and **twinject**.

### twinhook
twinhook is a DLL, injected during runtime into a Touhou game. It uses MS Detours to trampoline functions.

#### Procedure for DirectX Hooking
1. Hook `LoadLibraryA` with Detours, wait for `d3d9.dll` to be loaded
2. When `LoadLibraryA` hook is called and `d3d9.dll` is loaded, hook `Direct3DCreate9` with Detours based on procedure address
3. When `Direct3DCreate9` hook is called, return a dummy `IDirect3D9` that allows manipulation of device

#### Procedure for Bullet Location Reading
1. Hook function at 0x00431240, which is called whenever a bullet is processed
2. Every frame, store every bullet processed that frame into a list
3. Every new frame, wipe the list

#### Procedure for Powerup Reading
Repeat same process as abovee but hook function at 0x00440500

#### Procedure for Enemy Location Reading
... todo

#### [Bullet Update Routine](https://github.com/Netdex/twinject/blob/master/docs/bullet.md)

### twinject
twinject loads twinhook into th08.exe with Detours.

#### Procedure for DLL Injection
1. Start process thread in suspended state
2. Inject DLL with LoadLibraryA and CreateRemoteThread
3. Resume thread

OR

1. Use Detours to inject DLL
