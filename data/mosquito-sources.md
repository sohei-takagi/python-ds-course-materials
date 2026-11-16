# 飲み物と蚊の反応：実験データ

## 出典と利用条件

データ提供者：Dr Thierry Lefèvre。原著：Lefèvre, T., Gouagna, L.-C., Dabiré, K. R., Elguero, E., Fontenille, D., Renaud, F., Costantini, C., and Thomas, F. (2010). *Beer Consumption Increases Human Attractiveness to Malaria Mosquitoes*. PLOS ONE, 5(3), e9546. [原著](https://doi.org/10.1371/journal.pone.0009546)

[Matthew Brett による配布先](https://github.com/matthew-brett/datasets/tree/master/mosquito_beer)の README は，第一著者から直接提供されたデータであり，データと派生物を CC BY で利用できること，Thierry Lefèvre と原著への帰属を示すことを明記している。配布先が示す[ライセンス](https://creativecommons.org/licenses/by/4.0/)も併せて参照する。

2026-11-17 に取得した `processed/mosquito_beer.csv` を，ファイル名だけ `mosquito-experiment.csv` に変更して収録した。値，行順，列名は変更していない。43 人について飲用前後の各 1 回，合計 86 行・12 列。SHA-256 は `b08f854782499e26305add3f32688f84383b0a11036e7c172d4a64ad1a8fd5f3`。

## 授業で使う列

| 列 | 意味 |
|---|---|
| `volunteer` | 参加者の識別子。同じ人の飲用前後を対応させる。 |
| `group` | 割り当てられた飲み物。`beer` が 25 人，`water` が 18 人。 |
| `test` | `before` は飲用前，`after` は飲用 15 分後。 |
| `nb_released` | その測定で放した蚊の数。各回 50 匹。 |
| `no_odour` | 比較側のトラップで捕獲された数。 |
| `volunt_odour` | 参加者のにおい側のトラップで捕獲された数。 |
| `activated` | 二つのトラップの捕獲数の合計。 |

`activated / nb_released` は原著の activation に対応する。参加者のにおい側の割合である orientation とは異なる。捕獲数は刺された回数を表さない。残りの列は二つの経路の二酸化炭素濃度，体温，トラップの側，測定日時であり，原データの情報として残している。

## 本文の計算範囲

本文では飲用後の 43 行を使い，`activated` の平均差を比較する。合計はビール群 590 匹，水群 345 匹，平均はそれぞれ 23.6 匹と約 19.167 匹，差は約 4.433 匹。実習では人ごとに飲用前後を対応させ，変化量の群間差を調べる。蚊を独立した割付対象にせず，人のまとまりを保つ。

原著は無作為割付を記載しているが，割付を生成した手順の詳細までは示していない。本授業の置換計算では，43 人から 25 人を選ぶ各組が等確率という完全無作為化のモデルを置く。原著の一般化線形混合モデルは前後の反復測定や共変量も用いるため，本章はその検定結果の再現を目的としていない。

配布 CSV の最初の測定日時は 2007-08-28，最後は 2007-10-31。原著 Methods に記された実施期間（9〜10 月）と開始月に差がある。データの日時を改変せず残し，授業では日時による対象選択を行わない。
