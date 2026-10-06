WonderSwanColor Catalog, storage v4 (64 KiB blocks, 4 solid LZMA2 groups of up to 128 MiB). Metadata only: **no ROM payloads are published**; `compression_groups`, `chunks` and `object_chunks` are empty.

- Platform code `wsc` renamed to `wswanc` (Batocera system name); release tags now start with `wswanc-`.
- Naming normalized: platform codes are the Batocera system names, every populated database is `RetroBoxDB.<label>.sqlite`, and `meta.scope` / `meta.storage` are derived from the platform and the current storage parameters.
- One schema for all fifteen platforms: the header tables of every platform (including Master System, 32X, WonderSwan, NeoGeo Pocket and Pokémon Mini) and the provider-information tables exist in every Catalog; tables of other platforms and provider tables have no rows.
- RetroAchievements reports look up sibling databases (NES<->FDS, SNES<->Satellaview, WonderSwan<->WonderSwan Color, NeoGeo Pocket<->NeoGeo Pocket Color): a game whose ROM is stored there is `local_other_platform`, not a gap.
- Source: 310 ZIPs (nointro 265, retroachievements 45), 246.6 MiB (310 ROM files, 659.1 MiB uncompressed). Populated database: 126.5 MiB (51.3% of the ZIPs). All source ZIPs are reproduced byte-for-byte.
- Contents: 268 ROM records, 217 games, 253 releases; DAT versions: 20260525-011610.
- RetroAchievements: 33 of 33 games with achievements have a local ROM.
- Export (Intel(R) Core(TM) i7-8650U CPU @ 1.90GHz, idle, all checks): whole newest-DAT set with export_set.py 54.2 MiB/s (253 files); single file with a cold cache 1.226 s (ROM) / 1.398 s (TorrentZip) on average.
- Full audit of the populated database: 271 objects, 4 groups, 278 archive plans, no errors.

The release workflow starts from the base Catalog pinned by SHA256 in `release/catalog-release.json`, injects the engine and documents of the tagged commit, checks every data-table digest, SQLite integrity and foreign keys, runs the Catalog audit and the repository tests. Verify the download with `SHA256SUMS`.

[中文说明](https://github.com/rshi0212/RetroBoxDB-WonderSwanColor/blob/main/README.zh-CN.md)
