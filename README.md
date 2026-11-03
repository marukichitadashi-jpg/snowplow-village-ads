# SNOWPLOW VILLAGE RESORT — 掲示板の広告の置き場 / Ad boards

VRChat ワールド「SNOWPLOW VILLAGE RESORT」の掲示板に出す広告の絵と一覧です。ワールドは入った時（と 30 分ごと）にこの場所から読みこむので、ここのファイルを差しかえるだけで掲示板が変わります（ワールドの上げ直しは要りません）。

This repository holds the images and the list for the ad boards inside the VRChat world "SNOWPLOW VILLAGE RESORT". The world loads them when a player joins (and every 30 minutes), so changing files here changes the boards without re-uploading the world.

## ファイル / Files
- `ads.json` — 出す広告の一覧（どの枠に・いつからいつまで・何の広告か）/ the list of ads (which frame, period, title)
- `slots/<枠>_<番号>.png` — ワールドが読む絵。枠ごとに 1〜4 の固定の名前 / the images the world loads, fixed names per board slot
  - `entrance_1` … `entrance_4`（入口の広場 / entrance plaza — Tier 1）
  - `hub_1` … `hub_4`（宇宙ステーションのハブ / space station hub — Tier 2）
  - `hangar_1` … `hangar_4`（格納庫 / hangar — Tier 3）
  - `ski_1` … `ski_4`（スキー場のふもと / ski base — Tier 2・板は準備中 / board planned）
- `ads/*.png` — 絵の元（出稿者ごとの名前で保管）/ originals, kept by advertiser name. The world does not read this folder.

## なぜ絵の名前が固定なのか / Why the image names are fixed
VRChat の Udon は URL を実行中に組み立てられません（`VRCUrl` はワールドの組み立て時に決めた物だけ）。そこでワールドには `slots/<枠>_1〜4.png` の 16 個の URL をあらかじめ入れてあり、`ads.json` は「どの名前の絵を・いつ出すか」だけを書きます。絵を差しかえる時は **同じ名前で上書き** します。

Udon cannot build URLs at runtime (`VRCUrl` values are baked into the world). The world therefore knows the 16 URLs `slots/<slot>_1..4.png` in advance, and `ads.json` only says which of those names are shown and when. To change a picture, **overwrite the file with the same name**.

## 絵の決まり / Image rules
- PNG・**2048×1024**（横 2：縦 1）。1 枚 1.5MB まで / PNG, 2048×1024 (2:1), up to 1.5 MB each
- 英語を主に、日本語は補助 / English first, Japanese as a secondary line
- 自分の BOOTH・Jinxxy の商品だけ。QR は商品ページへ / your own BOOTH / Jinxxy items only; QR codes link to the item page
- VRChat のルールに合う物だけ（成人向け・実在の会社のロゴ・他人の作品は不可）/ must follow VRChat's rules (no adult content, no third-party logos, no other people's work)

## 広告を足す / Adding an ad
1. 絵の元を `ads/` に置く（例 `ads/2026-11_yourname.png`）/ keep the original in `ads/`
2. 出す枠の名前で `slots/` にコピーする（例 `slots/entrance_3.png`・空いている番号を使う）/ copy it into `slots/` under the frame name you sell (use a free number)
3. `ads.json` の `ads` に 1 つ足す / add one entry to `ads` in `ads.json`:
   ```json
   {"id": "2026-11_yourname", "title": "Your product", "link": "https://booth.pm/ja/items/0000000",
    "from": "2026-11-10", "to": "2026-11-17", "frames": ["entrance_3", "hub_3", "hangar_3"]}
   ```
4. `from`〜`to`（日本時間の日付）の外の日は出ません / ads outside `from`..`to` (Japan dates) are not shown
5. `frames` に書いた絵だけが読まれます。同じ絵を何枚もの板に出す時は、枠ごとに同じ名前の番号へコピーします / only the images named in `frames` are downloaded; to show one picture on several boards, copy it to the matching number in each slot

## 板の仕組み（ワールド側・SnowAdBoard）/ How the world reads it
- 一覧 `ads.json` を VRCStringDownloader で（入った 8 秒後に 1 回・以後 30 分ごと）、絵を VRCImageDownloader で 1 枚ずつ 5.5 秒あけて読む（`raw.githubusercontent.com` は VRChat の信頼された置き場所）
- 板は `interval_seconds` ごとに次の広告へ。となりの板とは別の広告から始まる / boards rotate every `interval_seconds`; neighbouring boards start on different ads
- 読めない時（失敗・一覧が空・期間外ばかり）は、ワールドに焼きこんである自分の商品の板のまま / if loading fails or nothing is active, the boards keep the built-in images
- 一度読んだ絵は、そのプレイヤーがワールドに入り直すまで読み直しません。絵を上書きした時は、入り直した人から新しい絵になります / a loaded image is not re-downloaded until the player rejoins; after overwriting a file, new joiners see the new picture

© まるきち / Marukichi. 絵の著作権はそれぞれの出稿者にあります / images belong to their advertisers.
