## Pure Data OWL Patches

OpenWare devices are capable of running Pure Data patches that have been compiled first with Heavy, then with the GCC ARM cross-compiler.

Currently Pd extensions are not supported, only (most) Pd Vanilla objects. See this [list of supported objects](SupportedObjects.md]. Notably `expr`, `expr~` and `vline` are not currently supported.

### Instructions
The easiest way to run a Pure Data patch on an OWL device is to use the online compiler. Go to [the patch library](https://www.rebeltech.org/patch-library/patches/my-patches/) and, if necessary, create an account. Then click on My Patches, Create patch, and upload all .pd files that your patch requires. Make sure to specify Compilation Type `pd`, and select the correct Main File (your top level Pd file).

You can also compile Pd patches offline using [OwlProgram](https://github.com/pingdynasty/OwlProgram) and a local installation of [hvcc](https://github.com/pingdynasty/hvcc.git) (the Heavy compiler; use our fork to take advantage of OWL-specific features and bug fixes). For details on using OwlProgram, see the project readme.

## Inputs and Outputs

### Parameters
There is a legacy and a recommended way to assign `[receive]` objects to specific OWL parameters. The legacy way is to use e.g. `[receive Channel-A]` to receive a value (between 0 and 1) from OWL parameter A. The recommended way is to use the `@owl` attribute.

    [receive NAME @owl PARAM MIN MAX DEFAULT]

receive a value called `NAME`, assigned to OWL parameter `PARAM`, in the range `MIN` to `MAX`, with default value `DEFAULT`.

example:

    [r Freq @owl A 220 880 440]

defines a receiver called `Freq`, assigned to OWL parameter `A`. The output is a float in the range 220.0 to 880.0 with default value 440.0 (`r` is shorthand for `receive`).
MIN, MAX and DEFAULT are optional. If omitted, MIN is 0, MAX is 1, and DEFAULT is calculated as the midway between the two. It is fine to declare only MIN, only MIN and MAX, all, or none.

The compiler supports up to 24 parameters, in three groups of eight, named from A to H, AA to AH and BA to BH, but the available hardware assignments vary depending on the OWL device. Magus has 20 CV inputs/outputs

Output parameters can be assigned in the same way, using `[send]` instead of `[receive]`. The compiler will add `>` to the end of the parameter name, to ensure it is recognised as an output.

### Buttons, Gates and Triggers
For hardware that supports input and output triggers, gates, and buttons, these can be assigned with the names `B1` trhrough `B8`. Output values are `0` for off, `1` for on. Any input value greater than `0.5` will be interpreted as on. 

### Examples

For a simple example patch that uses both input and output parameters and buttons, see e.g. this (Witch Pd Template)[https://www.rebeltech.org/patch-library/patch/Witch_Template].

## MIDI

Patches can send and receive MIDI messages with the usual Pd Vanilla MIDI I/O objects: `[notein]`, `[bendin]`, `[ctlin]`, `[pgmin]`, and `[noteout]`, `[bendout]`, `[ctlout]`, `[pgmout]`.
For an example see (this patch)[https://www.rebeltech.org/patch-library/patch/PD_MIDI].

## Known Bugs and Limitations

MIDI input objects don't filter when given initialisation arguments: `[notein]`,`[ctlin]` et c always produce *all* messages from *all* channels.
Raw MIDI receive and send with `[midiin]` and `[midiout]` is not yet supported.