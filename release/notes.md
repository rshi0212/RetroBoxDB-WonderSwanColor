WonderSwan Color Catalog, storage v4 (64 KiB blocks, 4 solid LZMA2 groups of up to 128 MiB). Metadata only: **no ROM payloads are published**; `compression_groups`, `chunks` and `object_chunks` are empty.

- First release: WonderSwan Color cartridges (16-byte footer) from the No-Intro folders including Aftermarket and the `.wsc` files of the shared RetroAchievements WonderSwan set; 1 Parent-Clone DAT version, DB Export and Dump Log unknown, RetroAchievements snapshot and Chinese names.
- Storage measured on the whole collection: 64 KiB blocks, 128 MiB groups (`assessment/data/storage-experiment-wsc.json`).
- RetroAchievements lists both platforms of the pair under one console and one folder; each database keeps its own files and looks up its sibling (WonderSwan<->WonderSwan Color).
- Source: 310 ZIPs (nointro 265, retroachievements 45), 246.6 MiB (310 ROM files, 659.1 MiB uncompressed). Populated database: 126.4 MiB (51.3% of the ZIPs). All source ZIPs are reproduced byte-for-byte.
- Contents: 268 ROM records, 217 games, 253 releases; DAT versions: 20260525-011610.
- RetroAchievements: 33 of 33 games with achievements have a local ROM.
- Chinese names: 115 of 242 CSV rows translated; 115 local ROMs with Chinese names.
- Export (Intel(R) Core(TM) i7-8650U CPU @ 1.90GHz, idle, all checks): whole newest-DAT set with export_set.py 54.2 MiB/s (253 files); single file with a cold cache 1.226 s (ROM) / 1.398 s (TorrentZip) on average.
- Full audit of the populated database: 271 objects, 4 groups, 278 archive plans, no errors.

The release workflow starts from the base Catalog pinned by SHA256 in `release/catalog-release.json`, injects the engine and documents of the tagged commit, checks every data-table digest, SQLite integrity and foreign keys, runs the Catalog audit and the repository tests. Verify the download with `SHA256SUMS`.

[中文说明](https://github.com/rshi0212/RetroBoxDB-WonderSwanColor/blob/main/README.zh-CN.md)
