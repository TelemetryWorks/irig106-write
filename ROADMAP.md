# Roadmap

Planned work is delivered in quick, small releases.

## Rewrite a recording with a corrected TMATS

Owner addition: 2026-11-27. Planned; not implemented.

- Write a **new** IRIG 106 Chapter 10 (Ch10) file; never modify the original recording.
- Replace the setup record with a user-supplied TMATS file.
- Read the input recording through `irig106-core`, using one reader, and read TMATS through `irig106-tmats`.
- Regenerate setup packets, including the Format 1 CSDW. Fragmentation is optional: a single Format 1 packet can be up to 134,217,728 bytes.
- Recompute sequence numbers and checksums only for channel 0: the regenerated setup packets, plus later channel-0 packets if the setup packet count changed. Subject to the Format 3 index decision below, copy every other packet byte-for-byte with its original sequence numbers and checksums; gaps are evidence of lost data.

### OPEN decisions

- **OPEN (owner) — Format 3 Recording Index offsets:** A different-size setup shifts later packets. Either regenerate the root/node index offsets and checksums, or drop the index and report it. Never copy it silently.
- **OPEN — Multiple setup records:** For recordings with multiple setup records (SRCC=1, §11.2.7.2, p. 11-50), decide whether to replace the first, replace all, or refuse, and how SRCC is set.
- **OPEN — CSDW fields and packet times:** FRMT is ASCII (`irig106-tmats` 0.1 has no XML). Decide whether RCCVER comes from the original CSDW or `G\106`, and choose the RTC / secondary-header time of the regenerated packets.
- **OPEN — Output safety:** Define how to refuse an output that resolves to the input, using the same rule as TMATS L3-CLI-034.
- **OPEN — Placement:** Decide between a library API over `Read`/`Write` and a CLI that owns files.
