# Encode transcript — CherokeeLessons#76 (the 45 source-backed tracks re-encoded at the 192k rung)

Source recovery: the pre-drop tree (`27191cf^` — parent of the CherokeeLessons#74 drop commit `27191cf feat(#74): drop the 45 superseded source mp3s, the mp3 zips, and the stale ignore rules — m4a is the site standard`), recovered via `git cat-file -p <drop-sha>^:<path>`; each recovered blob verified against its tree blob ID (`git hash-object` equality, 45/45) — the sha256 column re-verifies the byte content. Transient originals live in `/tmp/opencode/76-recovered/` (never added to git).

Toolchain: `.tools/ffmpeg` and `.tools/ffprobe` — static 7.0.x builds (AGENTS.md media-tooling record; `-nostdin` mandatory).

| # | file (basename) | source path | source sha256 | recovered-blob note |
|---|---|---|---|---|
| 1 | animals-0001.mp3 | content/content/cherokee-animal-names--ims-toucan-tts-/animals-0001.mp3 | 261b67e862403b3e8d88e7bdb8fb8c89294cbce3ba9609d0e4254f75a9d16dd4 | blob-ID match 45/45 |
| 2 | animals-0002.mp3 | content/content/cherokee-animal-names--ims-toucan-tts-/animals-0002.mp3 | 06f71900f5a050e46e31a1a6bad60d795b8a42e34fed79528c14610de915db04 | blob-ID match 45/45 |
| 3 | animals-0003.mp3 | content/content/cherokee-animal-names--ims-toucan-tts-/animals-0003.mp3 | 8666afe780bf13331f77d1c6136ab07571b27b44f5c7ed822c043dcfddb6b6c5 | blob-ID match 45/45 |
| 4 | animals-0004.mp3 | content/content/cherokee-animal-names--ims-toucan-tts-/animals-0004.mp3 | 1e89d9e2c453f3370f5b19502288a2f277dc9a3862dbaa93c1bdf4528e4876d3 | blob-ID match 45/45 |
| 5 | animals-0005.mp3 | content/content/cherokee-animal-names--ims-toucan-tts-/animals-0005.mp3 | 047919df620ffecba2b8189d57f9dfd4293b31b7d9015405e53581d971e8a463 | blob-ID match 45/45 |
| 6 | animals-0006.mp3 | content/content/cherokee-animal-names--ims-toucan-tts-/animals-0006.mp3 | f211a0c9bc8b3e87c301ced7bb339b7d80566197ef42b8bc91731214f83c6ae8 | blob-ID match 45/45 |
| 7 | animals-0007.mp3 | content/content/cherokee-animal-names--ims-toucan-tts-/animals-0007.mp3 | 327724a9bc0a9e4d39b553eaaf48e96144baed67e2031f2f811a501be3232b6a | blob-ID match 45/45 |
| 8 | animals-0008.mp3 | content/content/cherokee-animal-names--ims-toucan-tts-/animals-0008.mp3 | 511fcf3892d0aef1063445c70e5129147f98320793d87d12c2f79e53f7219948 | blob-ID match 45/45 |
| 9 | animals-0009.mp3 | content/content/cherokee-animal-names--ims-toucan-tts-/animals-0009.mp3 | ec7630775523de5453f287b0a8befb7ba86fb83c4e48882090ecbb19fcd1097e | blob-ID match 45/45 |
| 10 | Animals_2021-05-29_0001.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0001.mp3 | d31b4abce7ae1a608bab336e89805dee2b567384dbfb8c52546025e2b6a24bd6 | blob-ID match 45/45 |
| 11 | Animals_2021-05-29_0002.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0002.mp3 | c985a2604847f4f710119d9763a911c5ebb0dd11e3e7b3fb8824cc322b60fda7 | blob-ID match 45/45 |
| 12 | Animals_2021-05-29_0003.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0003.mp3 | 77b3cc3d4ea2d805e31828e57ebc6aefa89ac55e78386eb5225124ca3763805d | blob-ID match 45/45 |
| 13 | Animals_2021-05-29_0004.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0004.mp3 | bed61fa2d1372e5591749e3e6fced9232c5966f7971b9cccb66b557af5b94220 | blob-ID match 45/45 |
| 14 | Animals_2021-05-29_0005.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0005.mp3 | bdfbdf28a9ed98b1b5591295c32997f9c0846c606b6f76ec791bdd3b9c5bc975 | blob-ID match 45/45 |
| 15 | Animals_2021-05-29_0006.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0006.mp3 | 491fe337cb569856adcf0f68cc89272b5616bfb701b31492c4374df99218c018 | blob-ID match 45/45 |
| 16 | Animals_2021-05-29_0007.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0007.mp3 | 807731669117c91377d5e5a55603381f39d00b73b86fda2651d71b9bd474323e | blob-ID match 45/45 |
| 17 | Animals_2021-05-29_0008.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0008.mp3 | 7457be670f9ffa669048d849fd1539e07777afad7eb9d68350fbfd109fc3357f | blob-ID match 45/45 |
| 18 | Animals_2021-05-29_0009.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0009.mp3 | 45a2e854c737831cf071631427d98667af81f89ffe872d03fd259d7386ac5e13 | blob-ID match 45/45 |
| 19 | Animals_2021-05-29_0010.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0010.mp3 | c746f2fe76c20a18d39745d0366ef0898e2451699bc57072894d7f381caacb1a | blob-ID match 45/45 |
| 20 | Animals_2021-05-29_0011.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0011.mp3 | 4deef3fdd42fbc2c87e6ed15c2a88b3d17d1500237c70e331d40fb1cbbda5478 | blob-ID match 45/45 |
| 21 | Animals_2021-05-29_0012.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-29_0012.mp3 | 684aaed3f9e6ce8b47c753bc76c6de655ea2fe130a33dc72ba14ba30267fd1f2 | blob-ID match 45/45 |
| 22 | Animals_2021-05-31_0001.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0001.mp3 | 6f1fde2d22d831f6633e0e55fd3446e8d8928a3f9b4e00db2714d78d07dd980a | blob-ID match 45/45 |
| 23 | Animals_2021-05-31_0002.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0002.mp3 | b414aa48ce4fbd5d6cc27a36860a6f51fd622f668bafea9a57d880a95704263a | blob-ID match 45/45 |
| 24 | Animals_2021-05-31_0003.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0003.mp3 | 757acc13a6f69dc427619ffac7f40cdeb278fe6723021ab86e7f0aff79a47b05 | blob-ID match 45/45 |
| 25 | Animals_2021-05-31_0004.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0004.mp3 | 88e604ad94e8f7c7cc6396e136498729f3c1e37a0ca420d8ffa116d789b4444c | blob-ID match 45/45 |
| 26 | Animals_2021-05-31_0005.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0005.mp3 | 3dd789b17bbe3feaaba24777176e9d139c28f27ebd86ef595f5a3a2cfc1676ce | blob-ID match 45/45 |
| 27 | Animals_2021-05-31_0006.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0006.mp3 | 77c03c0cda1d04420484d618b408205db9f6844e001b05eb17f2769b9cd7b9d1 | blob-ID match 45/45 |
| 28 | Animals_2021-05-31_0007.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0007.mp3 | 963cfb54c86d835da91c9417fd06cc6fc93ecffbc47a82efe639008ddce0c70a | blob-ID match 45/45 |
| 29 | Animals_2021-05-31_0008.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0008.mp3 | 25c525e44ff1cfbaaed958927b0f1cdc10fe0f35dd52edf3cc4febcd193856da | blob-ID match 45/45 |
| 30 | Animals_2021-05-31_0009.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0009.mp3 | 7d64be0ab634177dbe030906536427c567c5e0671c25ea29a3f6532d64c7ff76 | blob-ID match 45/45 |
| 31 | Animals_2021-05-31_0010.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0010.mp3 | 530643908504a58a90045a6c0c186080267a80539fb5c60a6aa19fd0ea2f7348 | blob-ID match 45/45 |
| 32 | Animals_2021-05-31_0011.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0011.mp3 | c55c606a4c3b20683aef2d8ff03843e6a4feaf0a8c6e957d0ad00bc54ccd6857 | blob-ID match 45/45 |
| 33 | Animals_2021-05-31_0012.mp3 | content/content/cherokee-animal-names-tts-demo-audio-1/Animals_2021-05-31_0012.mp3 | 26e7118ca3805f976a9aa3857798cfbf440478ae6ed4a9e9fe4587eb59266f88 | blob-ID match 45/45 |
| 34 | bragging-hunter-en-360-m.mp3 | content/content/cherokee-tts-update-3--ims-toucan-/bragging-hunter-en-360-m.mp3 | eb24aae5332a22750461891af511877829f5ceaa16516b0fdac3117c85a09714 | blob-ID match 45/45 |
| 35 | bragging-hunter-en-361-f.mp3 | content/content/cherokee-tts-update-3--ims-toucan-/bragging-hunter-en-361-f.mp3 | 3be874dc3d2ce4c11849ed962dd767be35540f87ba0958ad34eaf2ddeae778a8 | blob-ID match 45/45 |
| 36 | search-party-en-360-m.mp3 | content/content/cherokee-tts-update-3--ims-toucan-/search-party-en-360-m.mp3 | 88dee62c5755482fed54f7ed46ed6a04060e99f6613bf19c0648e3bb1d669498 | blob-ID match 45/45 |
| 37 | search-party-en-361-f.mp3 | content/content/cherokee-tts-update-3--ims-toucan-/search-party-en-361-f.mp3 | 13728cac3d35e6cb388b710a075371d6322518cde16c65392c195fba92c9be83 | blob-ID match 45/45 |
| 38 | turtle-beat-rabbit-en-360-m.mp3 | content/content/cherokee-tts-update-3--ims-toucan-/turtle-beat-rabbit-en-360-m.mp3 | f4800b0abc43df04723ebf19c981f112241d08b3cd73d191c95d068a9cde3818 | blob-ID match 45/45 |
| 39 | turtle-beat-rabbit-en-361-f.mp3 | content/content/cherokee-tts-update-3--ims-toucan-/turtle-beat-rabbit-en-361-f.mp3 | c54fa9db0bfa9323daeeb8eb4caea983b8e304879a443c46bb45b46c1b95a9d0 | blob-ID match 45/45 |
| 40 | wolf-and-crawdad-en-360-m.mp3 | content/content/cherokee-tts-update-3--ims-toucan-/wolf-and-crawdad-en-360-m.mp3 | b53e9a148ee73a817da5cd4c26cfda9e5773e3f1b41a878d2e68f80bad759ef1 | blob-ID match 45/45 |
| 41 | wolf-and-crawdad-en-361-f.mp3 | content/content/cherokee-tts-update-3--ims-toucan-/wolf-and-crawdad-en-361-f.mp3 | 1daa8257d53fd95a543f8748dfaf17609e11370ee52346764b5bb9b140043a66 | blob-ID match 45/45 |
| 42 | oo1sdee3gaa_.mp3 | content/content/espeak-ng-and-ced-phonetics/oo1sdee3gaa_.mp3 | 27beb4a9e9dcfee3979d57c9c4be5b7742013152b75f6d7807d67c7feffa7ba0 | blob-ID match 45/45 |
| 43 | sdvv2goo2hvv23-ii_.mp3 | content/content/espeak-ng-and-ced-phonetics/sdvv2goo2hvv23-ii_.mp3 | d0d33f97ce5f1fa19e6c0e86f38d7fecddd3a50ffd085cbb4852057fa9818b37 | blob-ID match 45/45 |
| 44 | sgwoohl2gaa_.mp3 | content/content/espeak-ng-and-ced-phonetics/sgwoohl2gaa_.mp3 | acd090849842148df0f1900ed847b6fb27969d3fab320bfe52b5193cbe6f4350 | blob-ID match 45/45 |
| 45 | sgwoohl2gii_.mp3 | content/content/espeak-ng-and-ced-phonetics/sgwoohl2gii_.mp3 | e6260327c0b9e4cbdc4f3ae284624f1e53dcaf27998fea084f2c9cd87e7120cd | blob-ID match 45/45 |

## Encode rows (columns completed by item 2)

| # | file | rung(s) attempted | measured size(s) | outcome | command |
|---|---|---|---|---|---|
| 1 | animals-0001.mp3 | 192k | 7412959 | re-encoded at 192k, size=7412959 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 72856) |
| 2 | animals-0002.mp3 | 192k | 10274508 | re-encoded at 192k, size=10274508 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 74507) |
| 3 | animals-0003.mp3 | 192k | 10969904 | re-encoded at 192k, size=10969904 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 72644) |
| 4 | animals-0004.mp3 | 192k | 11856557 | re-encoded at 192k, size=11856557 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 70748) |
| 5 | animals-0005.mp3 | 192k | 14033043 | re-encoded at 192k, size=14033043 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 73087) |
| 6 | animals-0006.mp3 | 192k | 17628187 | re-encoded at 192k, size=17628187 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 84133) |
| 7 | animals-0007.mp3 | 192k | 18959810 | re-encoded at 192k, size=18959810 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 83280) |
| 8 | animals-0008.mp3 | 192k | 17673709 | re-encoded at 192k, size=17673709 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 83627) |
| 9 | animals-0009.mp3 | 192k | 12591755 | re-encoded at 192k, size=12591755 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 86048) |
| 10 | Animals_2021-05-29_0001.mp3 | 192k | 7416743 | re-encoded at 192k, size=7416743 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 62933) |
| 11 | Animals_2021-05-29_0002.mp3 | 192k | 10097605 | re-encoded at 192k, size=10097605 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 61780) |
| 12 | Animals_2021-05-29_0003.mp3 | 192k | 9833676 | re-encoded at 192k, size=9833676 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 60697) |
| 13 | Animals_2021-05-29_0004.mp3 | 192k | 9190705 | re-encoded at 192k, size=9190705 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 59058) |
| 14 | Animals_2021-05-29_0005.mp3 | 192k | 9690453 | re-encoded at 192k, size=9690453 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 61293) |
| 15 | Animals_2021-05-29_0006.mp3 | 192k | 14009717 | re-encoded at 192k, size=14009717 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 69815) |
| 16 | Animals_2021-05-29_0007.mp3 | 192k | 13960669 | re-encoded at 192k, size=13960669 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 67796) |
| 17 | Animals_2021-05-29_0008.mp3 | 192k | 12621680 | re-encoded at 192k, size=12621680 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 64407) |
| 18 | Animals_2021-05-29_0009.mp3 | 192k | 12474844 | re-encoded at 192k, size=12474844 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 63368) |
| 19 | Animals_2021-05-29_0010.mp3 | 192k | 13326185 | re-encoded at 192k, size=13326185 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 64397) |
| 20 | Animals_2021-05-29_0011.mp3 | 192k | 13769785 | re-encoded at 192k, size=13769785 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 65646) |
| 21 | Animals_2021-05-29_0012.mp3 | 192k | 10468987 | re-encoded at 192k, size=10468987 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 63057) |
| 22 | Animals_2021-05-31_0001.mp3 | 192k | 7416207 | re-encoded at 192k, size=7416207 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 62448) |
| 23 | Animals_2021-05-31_0002.mp3 | 192k | 10053723 | re-encoded at 192k, size=10053723 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 62625) |
| 24 | Animals_2021-05-31_0003.mp3 | 192k | 9848599 | re-encoded at 192k, size=9848599 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 61434) |
| 25 | Animals_2021-05-31_0004.mp3 | 192k | 9211731 | re-encoded at 192k, size=9211731 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 58362) |
| 26 | Animals_2021-05-31_0005.mp3 | 192k | 9699104 | re-encoded at 192k, size=9699104 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 60648) |
| 27 | Animals_2021-05-31_0006.mp3 | 192k | 13989600 | re-encoded at 192k, size=13989600 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 69716) |
| 28 | Animals_2021-05-31_0007.mp3 | 192k | 13956277 | re-encoded at 192k, size=13956277 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 67987) |
| 29 | Animals_2021-05-31_0008.mp3 | 192k | 12708417 | re-encoded at 192k, size=12708417 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 64602) |
| 30 | Animals_2021-05-31_0009.mp3 | 192k | 12463643 | re-encoded at 192k, size=12463643 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 63609) |
| 31 | Animals_2021-05-31_0010.mp3 | 192k | 13345503 | re-encoded at 192k, size=13345503 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 64211) |
| 32 | Animals_2021-05-31_0011.mp3 | 192k | 13852485 | re-encoded at 192k, size=13852485 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 66004) |
| 33 | Animals_2021-05-31_0012.mp3 | 192k | 10513222 | re-encoded at 192k, size=10513222 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 63630) |
| 34 | bragging-hunter-en-360-m.mp3 | 192k | 1067210 | re-encoded at 192k, size=1067210 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 158433) |
| 35 | bragging-hunter-en-361-f.mp3 | 192k | 1010032 | re-encoded at 192k, size=1010032 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 162663) |
| 36 | search-party-en-360-m.mp3 | 192k | 1880402 | re-encoded at 192k, size=1880402 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 156299) |
| 37 | search-party-en-361-f.mp3 | 192k | 1732542 | re-encoded at 192k, size=1732542 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 155940) |
| 38 | turtle-beat-rabbit-en-360-m.mp3 | 192k | 2699877 | re-encoded at 192k, size=2699877 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 159117) |
| 39 | turtle-beat-rabbit-en-361-f.mp3 | 192k | 2441602 | re-encoded at 192k, size=2441602 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 161324) |
| 40 | wolf-and-crawdad-en-360-m.mp3 | 192k | 2059215 | re-encoded at 192k, size=2059215 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 157846) |
| 41 | wolf-and-crawdad-en-361-f.mp3 | 192k | 1900972 | re-encoded at 192k, size=1900972 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 160495) |
| 42 | oo1sdee3gaa_.mp3 | 192k | 26753 | re-encoded at 192k, size=26753 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 140642) |
| 43 | sdvv2goo2hvv23-ii_.mp3 | 192k | 35640 | re-encoded at 192k, size=35640 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 154321) |
| 44 | sgwoohl2gaa_.mp3 | 192k | 19259 | re-encoded at 192k, size=19259 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 118444) |
| 45 | sgwoohl2gii_.mp3 | 192k | 20839 | re-encoded at 192k, size=20839 | `.tools/ffmpeg -y -nostdin -loglevel error -i IN -ac 1 -ar 48000 -c:a aac -b:a 192k -movflags +faststart OUT` (ffprobe bit_rate 128671) |
