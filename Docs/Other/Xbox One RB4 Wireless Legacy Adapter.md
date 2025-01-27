# Xbox One Rock Band 4 MadCatz Wireless Legacy Adapter

## Device Info

- Vendor ID: `0x0738`
- Product ID: `0x4164`
- Interface GUIDs:
  - Primary: `AF259D0F-76B0-4CDB-BFD1-CEA8C0A8F5EE`
  - Secondary:
    - `B8F31FE7-7386-40E9-A9F8-2F21263ACFB7` (Navigation)
    - `9776FF56-9BFD-4581-AD45-B645BBA526D6` (Unknown)
- Class strings:
  - Primary: `MadCatz.Xbox.Module.Brangus`
  - Secondary: `Windows.Xbox.Input.NavigationController`

## Input Command Info

### Command ID `0x20`: Input State

Length: Typically 14 bytes (listed max: 50 bytes)

- Bytes 0-1: 16-bit navigation button bitmask
  - These buttons are provided for compatibility with the Navigation Controller interface to allow navigation in the Xbox One menus.
  - Byte 0, bit 0 (`0x01`) - Sync button
  - Byte 0, bit 1 (`0x02`) - Unused
  - Byte 0, bit 2 (`0x04`) - Menu button
  - Byte 0, bit 3 (`0x08`) - View button
  - Byte 0, bit 4 (`0x10`) - A button
  - Byte 0, bit 5 (`0x20`) - B button
  - Byte 0, bit 6 (`0x40`) - X button
  - Byte 0, bit 7 (`0x80`) - Y button
  - Byte 1, bit 0 (`0x01`) - D-pad up
  - Byte 1, bit 1 (`0x02`) - D-pad down
  - Byte 1, bit 2 (`0x04`) - D-pad left
  - Byte 1, bit 3 (`0x08`) - D-pad right
  - Byte 1, bit 4 (`0x10`) - Left bumper
  - Byte 1, bit 5 (`0x20`) - Right bumper
  - Byte 1, bit 6 (`0x40`) - Left stick press
  - Byte 1, bit 7 (`0x80`) - Right stick press
- Byte 2: User index
- Byte 3: Unknown (`0x01`)
- Bytes 4-13: Instrument state
  - This seems to match the state layout of the respective Xbox One instruments, not the layout of Xbox 360 controllers.

```cpp
struct GipLegacyWirelessState
{
    bool sync : 1;
    bool : 1;
    bool menu : 1;
    bool view : 1;

    bool a : 1;
    bool b : 1;
    bool x : 1;
    bool y : 1;

    bool dpadUp : 1;
    bool dpadDown : 1;
    bool dpadLeft : 1;
    bool dpadRight : 1;

    bool leftShoulder : 1;
    bool rightShoulder : 1;
    bool leftThumbClick : 1;
    bool rightThumbClick : 1;

    uint8_t userIndex;
    uint8_t unk1 = 0x01;

    union {
        GipGuitarState guitar;
        GipDrumkitState drums;
        uint8_t raw[10];
    };
}
```

### Command ID `0x22`: Device Information

Max length: 254

- Byte 0: User index
- Byte 1: Type? (`0x01`)
- Bytes 2-3: Vendor ID (big-endian)
- Byte 4: Unknown (`0x00`)
- Byte 5: Subtype + `0x80`?
  - Guitar: `0x87`
  - Drums: `0x88`
- Byte 6 and onward: Little-endian wide-character name string
  - Guitar: `0x0067, 0x0075, 0x0069, 0x0074, 0x0061, 0x0072` (`L"guitar"`)
  - Drums: `0x0064, 0x0072, 0x0075, 0x006D, 0x0073` (`L"drums"`)

```cpp
struct GipLegacyWirelessDeviceInfo
{
    uint8_t userIndex;
    uint8_t type;
    uint16be_t vendorID;
    uint8_t unk;
    uint8_t subType;
    wchar_t[] name; // max length is 124
}
```

### Command ID `0x23`: Disconnection

Length: 1 byte

- Byte 0: Disconnected user index

```cpp
struct GipLegacyWirelessDisconnection
{
    uint8_t userIndex;
};
```

## Output Command Info

### Command ID `0x21`: Set State?

This is entirely speculative, needs testing.

Length: 2 bytes

- Byte 0: Left motor speed?
- Byte 1: Right motor speed?

```cpp
struct GipLegacyWirelessSetState
{
    uint8_t leftMotor;
    uint8_t rightMotor;
};
```

### Command ID `0x24`: Request Device Info

Length: 0 bytes

This request is sent to retrieve information about the connected device. The info is returned under command ID `0x22`.

## References

- https://rb4.app/js/app.js
