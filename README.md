# RetroBoxDB WonderSwanColor

English | [中文说明](README.zh-CN.md)

Single-file SQLite preservation database for Bandai WonderSwan Color. The public Catalog holds metadata only (checksums, DAT and provenance records, header fields, archive recipes and the processing code); it contains no ROM data and cannot restore files. The populated database stays local.

| Item | Value |
| --- | --- |
| Original size | 310 source ZIPs, 246.6 MiB (No-Intro 265, RetroAchievements sets 45); 310 ROM files, 659.1 MiB uncompressed |
| Stored size | populated database 126.5 MiB; public Catalog 3.9 MiB (no ROM data) |
| Ratio | 51.3% of the source ZIPs, 19.2% of the uncompressed ROM files |
| Technology | storage v4: SHA256-deduplicated 64 KiB blocks packed in No-Intro family order into solid LZMA2 groups of up to 128 MiB (128 MiB dictionary); per-block SHA256 and per-object CRC32/MD5/SHA1/SHA256 verification; source ZIPs reproduced byte-for-byte from TorrentZip plans |
| Export performance | Intel(R) Core(TM) i7-8650U CPU @ 1.90GHz, idle, Python 3.14.4, all checks included. whole newest-DAT set with `export_set.py` (253 files, each checked against the DAT hashes): 54.2 MiB/s, 37 ms per file on average; single file with a cold cache (the group is decoded up to the file): ROM 1.226 s, TorrentZip 1.398 s on average |

## Downloads and documents

| File / document | Content |
| --- | --- |
| [RetroBoxDB.WonderSwanColor.Catalog.sqlite](https://github.com/rshi0212/RetroBoxDB-WonderSwanColor/releases/latest/download/RetroBoxDB.WonderSwanColor.Catalog.sqlite) | Public Catalog (Release asset with `SHA256SUMS`) |
| [Storage v4 guide](RetroBoxDB.Storage-v4.en.md) / [中文](RetroBoxDB.Storage-v4.zh-CN.md) | Storage evaluation, contents, RA, names and maintenance for every platform |
| [Technical design](RetroBoxDB.Storage-v4.Technical-Design.en.md) | Storage format, platform adapters, incremental updates, verification |
| [RA list](reports/ra-wswanc-games.csv) / [summary](reports/ra-wswanc.json), [build report](reports/wswanc-build-report.json), [audit resolution](reports/audit-resolution-20261004.md) | Detailed data |

## Storage choice and platform specifics

18 block/group combinations measured on the whole local collection (`assessment/data/storage-experiment-wswanc.json`): smallest 64 KiB / 256 MiB at 121.78 MiB; by the rule (within 0.5% of the smallest, the smallest block, then the smallest group) 64 KiB / 128 MiB at 122.06 MiB. ZIPs 246.58 MiB, per-file LZMA 176.38 MiB.

- Footer: the last 16 bytes (far jump, publisher, colour flag, game id, version, ROM size, save type and size, orientation, bus width, RTC and the 16-bit sum of all other bytes) are stored in `ws_hardware`. WonderWitch homebrew carries a default footer with checksum 0, which accounts for most checksum warnings.
- RetroAchievements lists WonderSwan and WonderSwan Color under one console (53) and one folder. Both databases import that folder; this one keeps `.wsc` files and skips `.ws` files, which [RetroBoxDB-WonderSwan](https://github.com/rshi0212/RetroBoxDB-WonderSwan) holds. The RA report covers only games tied to this database.

## Contents

| Item | Value |
| --- | --- |
| ROM records / games / releases | 268 / 217 / 253 |
| DAT coverage per version | 20260525-011610: 253/253 |
| Local ROMs in no DAT | 15 |
| ROM files of the RetroAchievements set | in a No-Intro DAT 29, RA only 16, hash not in the latest RA snapshot 0 ([list](reports/ra-wswanc-collection-unknown.csv)); RA games still without a local ROM: [gap list](reports/ra-wswanc-missing.csv) |
| No-Intro DB Export + Dump Log unknown | 254 archives, 263 file identities, 263 documented hardware assertions; Dump Log Verified 53 |
| RetroAchievements (console 53) | 33 games with achievements: 33 with a local ROM (44 ROMs), 0 with the ROM in a sibling database, 0 DAT only, 0 DB file only, 0 without a No-Intro counterpart |
| Chinese names | 115 of 242 rows translated (97 unique); 115 local ROMs have a Chinese name |
| Populated-database audit | 271 objects, 4 groups, 278 archive plans, all passed |

Every source ZIP is reproduced byte-for-byte from its TorrentZip plan (`v_file_checksums.exported_bytes_equal_source`).

## Usage

```bash
# Query-only audit with the Catalog's embedded engine (also: stats, checksums FILE_ID, help)
python3 -B -c 'import sqlite3,sys; c=sqlite3.connect(sys.argv[1]); s=c.execute("SELECT content FROM resources WHERE name=?",("engine.py",)).fetchone()[0]; c.close(); exec(compile(s,"RetroBoxDB:engine.py","exec"))' ./RetroBoxDB.WonderSwanColor.Catalog.sqlite audit
# Populated database: export by DAT version, 1G1R, RA achievements, TorrentZip or plain ROMs
python3 -B tools/export_set.py RetroBoxDB.WonderSwanColor.sqlite OUT --set 1g1r --ra achievements --container torrentzip --layout ra-category
# Add new DATs, DB Export / Dump Log snapshots, ROMs and an RA snapshot incrementally
python3 -B tools/update_db.py RetroBoxDB.WonderSwanColor.sqlite --discover --ra --catalog RetroBoxDB.WonderSwanColor.Catalog.sqlite
```

Python 3.10+ standard library only. `engine.py` and the other `resources` entries are executable code; run them only from a database you built or a Release asset whose SHA256 you verified. Releases are produced by `.github/workflows/publish-catalog.yml`: it starts from the base Catalog pinned in `release/catalog-release.json`, injects the engine and documents of this commit, checks every data-table digest, runs the tests and the audit, then publishes.
