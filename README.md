# SNOWPLOW VILLAGE RESORT — 掲示板の広告の置き場 / Ad boards

VRChat ワールド「SNOWPLOW VILLAGE RESORT」の掲示板に出す広告の絵と一覧です。ワールドは入った時にこの場所から読みこむので、ここのファイルを差しかえるだけで掲示板が変わります（ワールドの上げ直しは要りません）。

This repository holds the images and the list for the ad boards inside the VRChat world "SNOWPLOW VILLAGE RESORT". The world loads them at start, so changing files here changes the boards without re-uploading the world.

## ファイル / Files
- `ads.json` — 出す広告の一覧（順番・期間・どの板に出すか）/ the list of ads (order, period, which boards)
- `ads/*.png` — 広告の絵 / ad images

## 絵の決まり / Image rules
- PNG・**2048×1024**（横 2：縦 1）。1 枚 1.5MB まで / PNG, 2048×1024 (2:1), up to 1.5 MB each
- 英語を主に、日本語は補助 / English first, Japanese as a secondary line
- 自分の BOOTH・Jinxxy の商品だけ。QR は商品ページへ / your own BOOTH / Jinxxy items only; QR codes link to the item page
- VRChat のルールに合う物だけ（成人向け・実在の会社のロゴ・他人の作品は不可）/ must follow VRChat's rules (no adult content, no third-party logos, no other people's work)

## 広告を足す / Adding an ad
1. 絵を `ads/` に置く（例 `ads/2026-11_yourname.png`）/ put the image in `ads/`
2. `ads.json` の `ads` に 1 つ足す / add one entry to `ads` in `ads.json`:
   ```json
   {"id": "2026-11_yourname", "image": "https://raw.githubusercontent.com/marukichitadashi-jpg/snowplow-village-ads/main/ads/2026-11_yourname.png",
    "title": "Your product", "link": "https://booth.pm/ja/items/0000000", "from": "2026-11-10", "to": "2026-11-17", "slots": ["entrance", "hub", "hangar"]}
   ```
3. `from`〜`to` の外の日は出ません / ads outside `from`..`to` are not shown
4. `slots`：`entrance`（入口の広場）・`hub`（宇宙ステーションのハブ）・`hangar`（格納庫）・`ski`（スキー場のふもと）/ board names

## 板の仕組み（ワールド側）/ How the world reads it
- 一覧 `ads.json` を VRCStringDownloader で、絵を VRCImageDownloader で読む（GitHub は VRChat の信頼された置き場所）
- 板は `interval_seconds` ごとに次の広告へ / boards rotate every `interval_seconds`
- 読めない時は、ワールドに焼きこんである自分の商品の板のまま / if loading fails, the boards keep the built-in images

© まるきち / Marukichi. 絵の著作権はそれぞれの出稿者にあります / images belong to their advertisers.
