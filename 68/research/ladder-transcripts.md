# Ladder transcripts — CherokeeLessons#68 R-1

Encoder: ffmpeg 7.0.2-static (johnvansickle.com static build, provisioned 2026-11-21 to repo .tools/, analysis-only)
Ladder: AAC-LC 128 -> 112 -> 96 kb/s, encode-and-measure per rung; rung accepted when encoded file <= 24 MiB (25,165,824 B)
Target: AAC-LC, mono, 48 kHz, MP4 container
Command per rung: ffmpeg -i <src> -c:a aac -b:a <R> -ac 1 -ar 48000 <dst>
Run started: 2026-11-21T15:16:09Z

| file | rungs attempted (kb/s: bytes) | chosen rung | final bytes |
|---|---|---|---|
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0001.mp3 | 128k:6207638  | 128k | 6207638 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0002.mp3 | 128k:8563830  | 128k | 8563830 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0003.mp3 | 128k:8270362  | 128k | 8270362 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0004.mp3 | 128k:7683444  | 128k | 7683444 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0005.mp3 | 128k:8198128  | 128k | 8198128 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0006.mp3 | 128k:11844261  | 128k | 11844261 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0007.mp3 | 128k:11804617  | 128k | 11804617 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0008.mp3 | 128k:10672745  | 128k | 10672745 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0009.mp3 | 128k:10538710  | 128k | 10538710 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0010.mp3 | 128k:11251328  | 128k | 11251328 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0011.mp3 | 128k:11534835  | 128k | 11534835 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0012.mp3 | 128k:8830132  | 128k | 8830132 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0001.mp3 | 128k:6200741  | 128k | 6200741 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0002.mp3 | 128k:8523935  | 128k | 8523935 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0003.mp3 | 128k:8279632  | 128k | 8279632 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0004.mp3 | 128k:7694239  | 128k | 7694239 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0005.mp3 | 128k:8208637  | 128k | 8208637 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0006.mp3 | 128k:11842232  | 128k | 11842232 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0007.mp3 | 128k:11811429  | 128k | 11811429 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0008.mp3 | 128k:10734947  | 128k | 10734947 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0009.mp3 | 128k:10539991  | 128k | 10539991 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0010.mp3 | 128k:11252632  | 128k | 11252632 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0011.mp3 | 128k:11592830  | 128k | 11592830 |
| content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0012.mp3 | 128k:8872699  | 128k | 8872699 |
| content/content/cherokee-animal-names--ims-toucan-tts-/animals-0001.mp3 | 128k:5917814  | 128k | 5917814 |
| content/content/cherokee-animal-names--ims-toucan-tts-/animals-0002.mp3 | 128k:8293982  | 128k | 8293982 |
| content/content/cherokee-animal-names--ims-toucan-tts-/animals-0003.mp3 | 128k:8859245  | 128k | 8859245 |
| content/content/cherokee-animal-names--ims-toucan-tts-/animals-0004.mp3 | 128k:9622787  | 128k | 9622787 |
| content/content/cherokee-animal-names--ims-toucan-tts-/animals-0005.mp3 | 128k:11492221  | 128k | 11492221 |
| content/content/cherokee-animal-names--ims-toucan-tts-/animals-0006.mp3 | 128k:14374763  | 128k | 14374763 |
| content/content/cherokee-animal-names--ims-toucan-tts-/animals-0007.mp3 | 128k:15248598  | 128k | 15248598 |
| content/content/cherokee-animal-names--ims-toucan-tts-/animals-0008.mp3 | 128k:13986160  | 128k | 13986160 |
| content/content/cherokee-animal-names--ims-toucan-tts-/animals-0009.mp3 | 128k:9976678  | 128k | 9976678 |
| content/content/cherokee-tts-update-3--ims-toucan-/bragging-hunter-en-360-m.mp3 | 128k:832147  | 128k | 832147 |
| content/content/cherokee-tts-update-3--ims-toucan-/bragging-hunter-en-361-f.mp3 | 128k:794495  | 128k | 794495 |
| content/content/cherokee-tts-update-3--ims-toucan-/search-party-en-360-m.mp3 | 128k:1448001  | 128k | 1448001 |
| content/content/cherokee-tts-update-3--ims-toucan-/search-party-en-361-f.mp3 | 128k:1359717  | 128k | 1359717 |
| content/content/cherokee-tts-update-3--ims-toucan-/turtle-beat-rabbit-en-360-m.mp3 | 128k:2097726  | 128k | 2097726 |
| content/content/cherokee-tts-update-3--ims-toucan-/turtle-beat-rabbit-en-361-f.mp3 | 128k:1897869  | 128k | 1897869 |
| content/content/cherokee-tts-update-3--ims-toucan-/wolf-and-crawdad-en-360-m.mp3 | 128k:1606295  | 128k | 1606295 |
| content/content/cherokee-tts-update-3--ims-toucan-/wolf-and-crawdad-en-361-f.mp3 | 128k:1475040  | 128k | 1475040 |
Run finished: 2026-11-21T15:21:20Z; failures: 0

## Rung-identification note (2026-11-21)

All 41 tracks accepted at the 128k rung (first rung; none needed 112/96 fallback; no track pre-lowered). Cross-check caveat: the ffmpeg native AAC encoder is average-bitrate — its achieved stream bit_rate is content-dependent and fell below the 128k target on quiet TTS speech with long silences (sampled: 51.9 / 66.7 / 122.8 kb/s). The chosen rung is identified by the encode command's `-b:a` target (recorded per row above), not by the achieved rate; `ffprobe` `bit_rate` therefore confirms the encode ran at the recorded rung's command, and no file was silently encoded at a lower target.
