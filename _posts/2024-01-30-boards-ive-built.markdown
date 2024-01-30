---
layout: post
title: "[Special] Boards I've Built"
data: 2023-01-30 21:00:00 +800
comments: true
categories: [Comic]
---

![](/MyBlog/images/boards/banner.jpg)

<br>

---

<br>2019年10月頃、PCBデザインを始めました。最初のプロジェクトは、[8MB PSRAM](https://cdn-shop.adafruit.com/product-files/4677/4677_esp-psram64_esp-psram64h_datasheet_en.pdf)を使ったPMOD拡張ボードです。未経験ですから、そのSOP-8チップだけで表面実装、ほかの抵抗器などはスルーホールになった。

![](/MyBlog/images/psram-pcb-final.png)

---

<br>

「7-Series Xilinx FPGAボードを作りたい」

Artix 7が高いため、ZYNQ-7000を選びました。初めての4層基板は、真面目にデザインした。SMT部品ができるだけ1206・0805サイズを使った。はんだペーストとフラックスに助けてもらった。PCBは五枚届いたか、四枚を手でコツコツ組立った。一枚は友達に贈ったり、一枚を売ったり、二枚は自分で使っています。

JTAG認証された感動な瞬間：

![](/MyBlog/images/boards/squeaky1.jpg)

![](/MyBlog/images/boards/squeaky-vivado.png)

写真ギャラリーとデザイン資料：https://github.com/regymm/SqueakyBoard

---

<br>

2020年初からチップ値上げ始めた。FT2232 USB JTAG/UART変換アダプタを作った。QFP実装はBGAより難しいじゃないか。

Vivadoで使えるように[ファームウェア書き替え](https://gist.github.com/rikka0w0/24b58b54473227502fa0334bbe75c3c1)が必要。法律的にちょっと微妙な技です。

![](/MyBlog/images/boards/ymmcu.jpg)

---

<br>

突然Altera MAX II CPLDを手に入れた。2層キーボード基板を作った。

![](/MyBlog/images/boards/epm.jpg)

---

<br>

[LCSC](https://www.lcsc.com)クーポンを無駄しないように、Lattice ICE40HX1K基板も。[OLIMEX社のPCBデザイン](https://github.com/OLIMEX/iCE40HX1K-EVB)です。Digilentステッカーも、ただでもらった。

![](/MyBlog/images/boards/ice1.jpg)

![](/MyBlog/images/boards/digilent.jpg)

---

<br>

BGAリボール。失敗と成功の例。

![](/MyBlog/images/boards/reball-1.jpg)

![](/MyBlog/images/boards/reball-2.jpg)

そして自作AD9363 SDR拡張ボード。

![](/MyBlog/images/boards/sdr-1.jpg)

調整完了したら、生産を試しました。初めてSMTサービスを使った。

![](/MyBlog/images/boards/sdr-2.jpg)

あとは2バッチも生産したが、なかなか売れなかった。

![](/MyBlog/images/boards/sdr-3.jpg)

---

<br>

とあるKintex-7ボードを拡張します。使いにくいと思います。結局一度でも開発しなかった（LED点滅だけ）。

![](/MyBlog/images/boards/k7-1.jpg)

---

<br>

二層Xilinx ZYNQ 7000 CLG400を作った。抵抗器７つと電源３路だけでJTAG書き込みできます。ZYNQ失格。

![](/MyBlog/images/boards/2lz.jpg)

裏面は。。。

![](/MyBlog/images/boards/2lz-1.jpg)

信号品質悪すぎて、JTAG 30MHzもできなかったが、480p VGA出力できます。

---

<br>

デザインが一年かかった６層のXilinx ZYNQ Ultrascale+ ZU15EG、届いた。只今調整中です。

![](/MyBlog/images/boards/15eg.jpg)

![](/MyBlog/images/boards/15eg-1.jpg)
