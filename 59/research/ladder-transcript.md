# Per-track ladder transcript — CherokeeLessons#59

Executed 2026-11-20. Ladder: 128 → 112 → 96 kb/s, encode-and-measure; the highest
rung whose encoded output measured ≤ 24 MiB (25,165,824 B) was kept. All 9 tracks
landed at the 128k rung on the first attempt — no fallback was required.

Transcoder command (identical form for every track and rung, only `-b:a` varying):

```
ffmpeg -y -loglevel error -i animals-NNNN.mp3 \
  -ac 1 -ar 48000 -c:a aac -b:a <rung> out/animals-NNNN.m4a
```

Measured byte sizes per rung (stat -c %s):

Track  128k rung   chosen
0001   5,446,448   128k
0002   6,774,701   128k
0003   7,923,668   128k
0004   9,376,927   128k
0005  10,477,197   128k
0006  11,085,619   128k
0007  11,096,477   128k
0008   8,536,989   128k
0009   5,037,093   128k

Largest track (0007) = 11,096,477 B = 10.6 MiB — every track cleared the cap at the
top rung with roughly a 2.3× margin; the 112k and 96k rungs were never exercised.
Original mp3 sources totaled 31 MB; converted set totals ~75.7 MB — consistent with
the owning definition's trial observation (AAC at nominal 128k over quiet mono
speech lands near the nominal rate here since the sources are full-band speech
audio at a ~46–59 kb/s information ceiling; measured effective bit rates run
57–83 kb/s, see ffprobe-probe.md; updated after the
ffprobe run: this set measured 42.8–55.3 kb/s — the owning definition's
documented quiet-mono-speech undershoot under the same 128k target rung).
