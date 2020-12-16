---
title : "PC"
date : 2019-09-03
publishdate : 2019-09-03
weight : 10
type: docs
---

# PC

# CPU

[4Gamer: Zen 3アーキテクチャ採用の新世代CPUはゲームにおける性能が大きく向上した: Ryzen 9 5900X, Ryzen 7 5800X](https://www.4gamer.net/games/461/G046172/20201105003/)


# メモリ規格

[見れば全部わかるDDR4メモリ完全ガイド、規格からレイテンシ、本当の速さまで再確認](https://akiba-pc.watch.impress.co.jp/docs/sp/1231939.html)

[オーバークロックメモリの基本と実際の性能、見れば全部わかるDDR4メモリ完全ガイド](https://akiba-pc.watch.impress.co.jp/docs/sp/1234824.html)

sigle rank, dual rank

http://blog.livedoor.jp/ocworks/archives/52125318.html

DDR4 メモリ規格

DIMM デスクトップ用メモリ、
SO-DIMM ノートパソコン用メモリ

RDIMM/UDIMM メモリにレジスタを備えているか、いないか、RDIMMはサーバー用（マザボ）、一般的なやつはUDIMM

ECC/non-ECC メモリのエラーチェック機能、CPUやマザボ側も対応している必要がある。基本的にはサーバー向けの機能

メモリクロック
DDR4−3200 = 3200 MHz データ転送レート
クロック信号の周波数(DRAM frequency) ＝データ転送レートの半分 1600 MHz

メモリの帯域幅 
PC4-25600 １秒間に転送可能なデータ量
25600 MB/s

メモリタイミング、レイテンシ
16-18-18-38
先頭の数字 CASレイテンシ CL16と表記されることもある
CL16 = メモリコントローラの要求に対してメモリが応答するまで16クロック分の時間がかかる
小さい方が早い

メモリクロックは1600 MHz

1クロック  = 1/1600,000,000 秒
16クロックは 16/1600,000,000 = 1/100,000,000 秒 = 10ns

メモリランク
  チップ構成、シングルランク、デュアルランク、シングルの方が良いらしい

SPDとXMP
SPDとは、Serial Presence Detect
2666MHz
1.20V
メモリの仕様をシステム側に伝えるための仕組み、何もしないとこのUEFI側はこの仕様でメモリを動作させる

メモリがXMPに対応していると、UEFIででXMPを有効化すると、自動でXMPデータに基づく動作設定（オーバークロック設定）が適用される


SPDは標準仕様に準拠した設定（デフォルトではこちらの設定で動作する）、XMPを有効にするとオーバークロック設定で動作する
XMPはインテルの技術だが、AMDマザボでもマザボメーカーの努力により利用可能


# 部品

- [X570 AORUS ELITE (rev. 1.0)](https://www.gigabyte.com/jp/Motherboard/X570-AORUS-ELITE-rev-10#kf)
- [F4-3200C16Q-128GVK](https://www.gskill.com/product/165/184/1571734065/F4-3200C16Q-128GVK-Overview)
- [Ryzen 5900X](https://www.amd.com/ja/products/cpu/amd-ryzen-9-5900x)
- [ZOTAC GAMING GeForce RTX 3080 Trinity](https://www.zotac.com/jp/product/graphics_card/zotac-gaming-geforce-rtx-3080-trinity)

- [plextor px-512m9pey](https://www.links.co.jp/item/m9pey/)
- [Kingston SSD A2000 1000GB 1TB M.2 2280 NVMe PCIe 3D TLC](https://www.kingston.com/jp/ssd/a2000-nvme-pcie-ssd)
- [Intel SSD 660p Series [M.2 PCI-E SSD 1TB]](https://www.intel.co.jp/content/www/jp/ja/products/memory-storage/solid-state-drives/consumer-ssds/6-series/ssd-660p-series.html)

```
CPU      : AMD Ryzen 9 3900X Matisse [3.8GHz/12Core/TDP105W] 搭載モデル（標準構成価格220,620円）
CPUグリス: Noctua NT-H1 [マイクロ粒子ハイブリッド化合物、高熱伝導率タイプ]（+2,040円）
CPU-FAN  : サイコムオリジナルAsetek 670LS + Enermax UCTB12P x2 [240mm水冷ユニット] ※メンテナンスフリー（+7,460円）
MOTHER   : GIGABYTE X570 AORUS ELITE [AMD X570chipset]（標準）
MEMORY   : 64GB[16GB*4枚] DDR4-3200 [メジャーチップ・8層基板] Dual Channel（+28,550円）
READER   : なし（標準）
HDD/SSD  : Intel SSD 660p Series [M.2 PCI-E SSD 1TB]（+8,730円）
SSD-Option: なし（標準）
HDD/SSD2 : TOSHIBA MD05ACA800 [高信頼 8TB 7200rpm 128MB]（+26,330円）
OptDrive : なし（-2,050円）
VGA      : なし（Ryzen はオンボードグラフィック非対応）（-44,900円）
ExCard   : オンボードサウンド（標準）
LAN      : Gigabit LAN [1000BASE T] オンボード
CASE     : 【黒】CoolerMaster CM694（標準）
CASE-Option: なし（標準）
POWER    : SilverStone SST-ST75F-GS V3 [750W/80PLUS Gold]★フルモジュラー高品質電源がお買い得！★（標準）
OS       : Microsoft(R) Windows10 Home (64bit) DSP版（標準）
```