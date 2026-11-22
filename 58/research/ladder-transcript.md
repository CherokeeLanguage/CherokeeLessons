# Per-track ladder transcript — CherokeeLessons#58

Executed 2026-11-21. Ladder: 128 → 112 → 96 kb/s encode-and-measure; the highest
rung whose encoded output measured ≤ 24 MiB (25,165,824 B) was kept. All 23 tracks
landed at the 128k rung on the first attempt — no fallback was required (consistent
with the owning definition audio-lessons-generator-python#16's trial bound: worst
case ~20 MB at the 96k floor; here the worst track measured 25,004,230 B ≈ 23.84 MiB
at the top rung, inside the 24 MiB target with ~0.16 MiB margin).

Tool provenance: `.tools/ffmpeg` / `.tools/ffprobe`, static 7.0.2 builds
(johnvansickle.com release, vendored per AGENTS.md — `/snap/bin/ffmpeg` is broken in
this environment and was not used). `-nostdin` always passed (the #67 stdin-hang
guard).

Transcoder command (identical form for every track; only `-b:a` varied — the 112k
and 96k rungs were armed but never exercised):

```
.tools/ffmpeg -y -nostdin -loglevel error -i cll1-v3-NNNN.mp3 \
  -ac 1 -ar 48000 -c:a aac -b:a 128k cll1-v3-NNNN.m4a
```

Measured byte sizes (stat -c %s), all at the chosen 128k rung:

| Track | Encoded size (B) | Chosen rung | ffprobe bit_rate |
|---|---|---|---|
| 0001 | 6,003,030 | 128k | 56,696 |
| 0002 | 6,261,272 | 128k | 43,938 |
| 0003 | 7,828,085 | 128k | 46,352 |
| 0004 | 9,154,941 | 128k | 47,021 |
| 0005 | 9,670,937 | 128k | 46,962 |
| 0006 | 12,715,869 | 128k | 50,576 |
| 0007 | 14,660,533 | 128k | 51,400 |
| 0008 | 16,084,216 | 128k | 50,692 |
| 0009 | 16,342,364 | 128k | 49,432 |
| 0010 | 15,476,605 | 128k | 49,756 |
| 0011 | 19,760,691 | 128k | 54,585 |
| 0012 | 18,591,408 | 128k | 49,347 |
| 0013 | 18,581,333 | 128k | 48,208 |
| 0014 | 24,350,149 | 128k | 52,834 |
| 0015 | 20,508,521 | 128k | 50,693 |
| 0016 | 17,395,862 | 128k | 50,421 |
| 0017 | 22,385,475 | 128k | 51,218 |
| 0018 | 24,038,342 | 128k | 52,252 |
| 0019 | 24,864,419 | 128k | 54,111 |
| 0020 | 24,407,173 | 128k | 53,115 |
| 0021 | 24,224,522 | 128k | 52,597 |
| 0022 | 25,004,230 | 128k | 54,362 |
| 0023 | 23,341,387 | 128k | 50,547 |

Every file probed: codec_name=aac (AAC-LC), channels=1 (mono), sample_rate=48000,
format_name contains mov/mp4. Set total ~323 MB vs 419 MB of retired mp3 sources.
Effective bit rates 44–57 kb/s — the owning definition's quiet-mono-speech
undershoot pattern (nominal 128k target, information ceiling below nominal).
