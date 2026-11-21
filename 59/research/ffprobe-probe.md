# Tool provenance + ffprobe evidence — CherokeeLessons#59

## Toolchain

- Source spec row (Dependencies): `ffmpeg` present, `ffprobe` absent, "provisioned
  at implement time". Provisioning executed 2026-11-20.
- The environment's ffmpeg is an Ubuntu 26.04 snap (`/snap/bin/ffmpeg`); snap
  confinement is broken on this machine (snap-confine capability error,
  `cap_dac_override` unavailable; sudo blocked by no-new-privileges) — the snap
  binary is unusable on this environment.
- Provisioned instead from the static build provider johnvansickle.com:
  `ffmpeg-release-amd64-static` → `ffmpeg-7.0.2-amd64-static`
  (`ffmpeg version 7.0.2-static`, `ffprobe version 7.0.2-static`), unpacked under
  `/tmp/opencode/ffbin/` (session-scoped, not committed). This is analysis/
  media tooling provisioning, not a build-framework substitution.
- SC-3/SC-4 ffprobe instrument: `/tmp/opencode/ffbin/ffmpeg-7.0.2-amd64-static/ffprobe`.

## ffprobe per-file results (SC-3 / SC-4 cross-check) — executed 2026-11-20

Instrument: `ffprobe -v error -show_entries stream=codec_name,channels,sample_rate,bit_rate -show_entries format=format_name,size -of json animals-NNNN.m4a`

| Track | codec | channels | sample_rate | bit_rate (measured) | format_name             | size (B)   |
|-------|-------|----------|-------------|---------------------|-------------------------|------------|
| 0001  | aac   | 1        | 48000       | 55,300              | mov,mp4,m4a,3gp,3g2,mj2 | 5,446,448  |
| 0002  | aac   | 1        | 48000       | 43,502              | mov,mp4,m4a,3gp,3g2,mj2 | 6,774,701  |
| 0003  | aac   | 1        | 48000       | 42,769              | mov,mp4,m4a,3gp,3g2,mj2 | 7,923,668  |
| 0004  | aac   | 1        | 48000       | 43,468              | mov,mp4,m4a,3gp,3g2,mj2 | 9,376,927  |
| 0005  | aac   | 1        | 48000       | 45,372              | mov,mp4,m4a,3gp,3g2,mj2 | 10,477,197 |
| 0006  | aac   | 1        | 48000       | 48,000              | mov,mp4,m4a,3gp,3g2,mj2 | 11,085,619 |
| 0007  | aac   | 1        | 48000       | 48,037              | mov,mp4,m4a,3gp,3g2,mj2 | 11,096,477 |
| 0008  | aac   | 1        | 48000       | 44,990              | mov,mp4,m4a,3gp,3g2,mj2 | 8,536,989  |
| 0009  | aac   | 1        | 48000       | 45,527              | mov,mp4,m4a,3gp,3g2,mj2 | 5,037,093  |

Every file: AAC-LC (`codec_name` aac), mono (channels 1), 48 kHz (`sample_rate`
48000), `format_name` contains mov/mp4, size ≤ 25,165,824 B. SC-3 passes per file.

SC-4 cross-check note: the encoder target rung for all 9 tracks was 128k (per
`ladder-transcript.md` — every track cleared the 24 MiB cap on the first rung), but
ffprobe's measured `bit_rate` runs 42.8–55.3 kb/s. This is the quiet-mono-speech
undershoot the owning definition (audio-lessons-generator-python#16, 2026-11-15
trials: effective ~46–59 kb/s, source information ceiling ~57 kb/s) already
documents — the AAC encoder targets the nominal rate but the signal cannot absorb
it. Consistent with the spec's Key Design Decision (1): final encoded size, not
nominal rate, decides the rung. No track's probe contradicts its transcript: the
transcripts record the attempted rung, the measured size at that rung (every
measurement ≤ cap), and the chosen rung — and each file's measured size matches
its transcript row exactly.
