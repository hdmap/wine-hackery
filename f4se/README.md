These are patches to get Fallout 4 Script Extender working in wine. f4se allocates memory for trampolines in the spaces before the Fallout4 exe image and the f4se dll image. These patches: 

1) Fix VirtualQuery rejecting any blocks before the image as already allocated (it looks like get_free_mem_state_callback sees blocks before the exe base address as overlapping the exe base from calculating the block end address as block start address + size, ie a 256 byte block starting at 0x100 would "end" at 0x200).

2) Switches dlls not loaded at their preferred address to be loaded at high addresses rather than low ones to work around code in f4se that gives up allocating the trampolines (it compares the address being queried to a lowest acceptable address computed by subtracting 0x78000000 from the module address, which will wrap around and fail with lower addresses even if there's free space).

Apparently these patches also get SkyrimSE script extender working, but I don't own that so I can't test that.
