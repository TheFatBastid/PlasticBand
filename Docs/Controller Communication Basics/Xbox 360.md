# Communicating with Xbox 360 Controllers

## Windows

### XInput

On Windows, Xbox 360 controllers are typically used by programs through the XInput API.

Windows 8 and onward include XInput with the system as `xinput1_4.dll`, along with `xinput9_1_0.dll` for backwards compatibility. Windows 7 includes it as just `xinput9_1_0.dll`. It is also available in the DirectX SDK as `xinput1_3.dll`, `xinput1_2.dll`, and `xinput1_1.dll`.

An example of using XInput:

```cpp
#include <Xinput.h>

for (int userIndex = 0; userIndex < XUSER_MAX_COUNT; userIndex++)
{
    XINPUT_STATE state;
    if (XInputGetState(userIndex, &state) != ERROR_SUCCESS)
    {
        // Processing of a disconnected controller goes here
        continue;
    }

    // Input processing code goes here
}
```

XInput 1.4 and 1.3 include a number of hidden exports which provide additional functionality. These exported functions can be called by loading the respective XInput library and getting the address for the export by ordinal:

```cpp
#include <Windows.h>
#include <Xinput.h>

// This function is the exact same as XInputGetState, except it doesn't mask off the guide button in the state.
// It's present at ordinal #100.
typedef DWORD (WINAPI* XINPUT_GET_STATE_EX)(DWORD userIndex, XINPUT_STATE* pState);
#define XINPUT_GET_STATE_EX_ORDINAL (LPSTR)100

HMODULE hXInput = LoadLibrary(TEXT("xinput1_4.dll"));
if (hXInput)
{
    XINPUT_GET_STATE_EX pfnXInputGetStateEx = (XINPUT_GET_STATE_EX)GetProcAddress(hXInput, XINPUT_GET_STATE_EX_ORDINAL);
    if (pfnXInputGetStateEx)
    {
        XINPUT_STATE state;
        if (pfnXInputGetStateEx(userIndex, &state) != ERROR_SUCCESS)
        {
            // ...
        }
    }
}
```

For the other hidden exports, see [OpenXInput](https://github.com/Nemirtingas/OpenXinput)'s header and .def file.

### Windows.Gaming.Input

The Universal Windows Platform `Windows.Gaming.Input` namespace also lets you use Xbox 360 controllers, among other things. However, WGI doesn't just hand you the input data as-is: axis values are normalized to floats, which doesn't work for devices that use those axes as bitmasks. Additionally, not all Xbox 360 subtypes are supported by it.

There is also the `Windows.Gaming.Input.Custom` namespace, however there is no official documentation on how to use it, and all of my attempts to use it have failed so far.

### Other Options

XInput works just fine for getting input data from an Xbox 360 controller: it does little to no processing on it. But it has some limitations, and there's some functionality it doesn't allow, and data that it doesn't expose that some devices might use.

- The [OpenXInput](https://github.com/Nemirtingas/OpenXinput) project is a reverse-engineering/re-implementation of XInput which allows using more than 4 controllers at once, and has some additional functionality.
- My own [SharpXusb](https://github.com/TheNathannator/SharpXusb) library for .NET allows for getting data nearly directly from the driver, which means some additional input data that would otherwise get lost can be used.

## Mac

On the latest versions of macOS, there is currently no way to use Xbox 360 controllers. Previously, there was an [open-source Xbox 360 controller driver](https://github.com/360Controller/360Controller) that enabled using it via HID, but this driver no longer works due to kernel extensions being deprecated and unsupported since macOS Big Sur. The maintainers of that driver have no plans to update it unfortunately.

## Linux

On Linux, Xbox 360 controller support is built directly into the kernel. There are also alternative drivers which provide additional functionality and may work better with some controllers:

- [xboxdrv](https://gitlab.com/xboxdrv/xboxdrv)
- a modified [xpad](https://github.com/paroj/xpad)
- [xpad-noone](https://github.com/medusalix/xpad-noone) (required if using [xone](https://github.com/medusalix/xone))

(TODO: how to identify and interface with devices)
