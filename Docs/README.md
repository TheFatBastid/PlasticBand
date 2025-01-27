# Rock Band and Guitar Hero Peripheral Documentation

This folder contains documentation on how to communicate with and interpret the data of RB/GH peripherals.

## Contributions

These docs are a work in progress, contributions are very welcome!

To get started, use one of the templates in the [_Templates](_Templates/) folder. Fill out information required to identify the device, include any device descriptors if possible, and give a writeup on how its input data works. Code examples (including data structures) are also appreciated. If you're missing info, be sure to make note of such. Descriptor dumps can be added to the [Descriptor Dumps](Descriptor%20Dumps/) folder.

You may add images to documentation, and you may also create an `_Ignored` folder anywhere within the repository to have a folder you can put anything in without fear of accidentally committing it.

## General Notes

Data structures in these documents assume that the processor and data are little-endian, and that the structure is unaligned with no packing applied, unless otherwise specified. If you use them directly, you'll want to ensure these assumptions are correct, or that you adjust the structures to make them correct on your system.

## Todo List

- 5-fret guitar
  - Guitar Hero
    - [x] Xbox 360
    - [ ] PS2
      - This is assuming they can be differentiated from regular PS3 controllers at all, and hoping the PS2 adapter landscape isn't too fragmented. Chances are they will require manual mapping, though.
    - [x] PS3
    - [ ] Wii
  - Rock Band
    - [x] Xbox 360
    - [x] Xbox One
      - [x] Stratocaster
      - [ ] Jaguar
    - [x] PS2/3
    - [x] PS4
    - [x] Wii
- 6-fret guitar
  - [x] Xbox 360
  - [x] Xbox One
  - [x] PS3/Wii U
  - [x] PS4
  - [x] iOS
- 4-lane (Rock Band) drums
  - [x] Xbox 360
  - [x] Xbox One
  - [x] PS2/3
  - [x] PS4
  - [x] Wii
- 5-lane (Guitar Hero) drums
  - [x] Xbox 360
  - [x] PS2/3
  - [ ] Wii
- Turntable (DJ Hero)
  - [x] Xbox 360
  - [x] PS3
  - [ ] Wii
- Pro Guitar (Rock Band)
  - [x] Xbox 360
  - [x] PS3
  - [x] Wii
- Keyboard/keytar (Rock Band)
  - [x] Xbox 360
  - [ ] PS3
  - [x] Wii
- Other
  - [x] Rock Band 4 Legacy Wireless Adapter
  - [x] Rock Band 4 Legacy Wired Adapter
  - [x] Xbox 360 Rock Band 2 Stage Kit

## License

[![Creative Commons Attribution-ShareAlike 4.0 International License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-sa/4.0/)

This documentation is licensed under the [Creative Commons Attribution-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-sa/4.0/). See the Creative Commons website for details.
