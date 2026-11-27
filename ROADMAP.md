# Roadmap

Planned work is delivered in quick, small releases.

## Rewrite a recording with a corrected TMATS

Owner addition: 2026-11-27. Planned; not implemented.

- Write a **new** IRIG 106 Chapter 10 (Ch10) file; never modify the original recording.
- Replace the setup record with a user-supplied TMATS file.
- Read the input recording through `irig106-core`, using one reader, and read TMATS through `irig106-tmats`.
- Regenerate setup packets, including the Format 1 CSDW and fragmentation.
- Recompute checksums and sequence numbers for the output.
