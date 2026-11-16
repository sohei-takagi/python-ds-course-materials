# Palmer Penguins の出典

`penguins.csv` は，Palmer Station LTER のペンギン調査を利用しやすい 8 列に整理した `palmerpenguins` の CSV である。人工生成したデータではない。配布元の値・列・行順を変更せず収録した。

## 配布元と版

- データ整理：Allison Horst，Alison Hill，Kristen Gorman。調査：Kristen Gorman と Palmer Station LTER の研究者。
- [プロジェクトの説明](https://allisonhorst.github.io/palmerpenguins/)
- [取得した版の CSV](https://raw.githubusercontent.com/allisonhorst/palmerpenguins/8957207b78d6ccd1b4654a9dd9c9041b657478ab/inst/extdata/penguins.csv)
- 配布元コミット：`8957207b78d6ccd1b4654a9dd9c9041b657478ab`。確認日：2026-11-16。
- 収録ファイルの SHA-256：`f204db2c753b0937caac3cb35258562c14f073e4bbc76be24b4c51ce22767a93`。
- [配布元のライセンス](https://allisonhorst.github.io/palmerpenguins/LICENSE.html)：CC0 1.0。

## 記録の読み方

全 344 行，8 列。1 行は調査された成鳥の記録である。`year` は 2007，2008，2009。調査は南極半島付近の Biscoe，Dream，Torgersen の 3 島で行われた。

| 列 | 内容 |
|---|---|
| `species` | ペンギンの種類：Adelie，Chinstrap，Gentoo。 |
| `island` | 調査した島。 |
| `bill_length_mm` | くちばしの長さ（mm）。 |
| `bill_depth_mm` | くちばしの厚み（mm）。 |
| `flipper_length_mm` | 翼の長さ（mm）。 |
| `body_mass_g` | 体重（g）。 |
| `sex` | 性別。 |
| `year` | 調査年。 |

4 測定列は同じ 2 行に欠損があり，測定値がそろうのは 342 行。`sex` の欠損は 11 行である。第 7 章の比較は性別を使わないので，性別の欠損だけを理由に記録を除かない。CSV の `NA` は欠損の印であり，0 に置き換えない。

| ペンギンの種類 | 全記録 | 体重の記録がある行 |
|---|---:|---:|
| Adelie | 152 | 151 |
| Chinstrap | 68 | 68 |
| Gentoo | 124 | 123 |

## 調査の範囲

原著の調査対象は繁殖中の成鳥である。ペンギンの種類によって調査した島が異なり，この CSV でも Chinstrap は Dream，Gentoo は Biscoe の記録に限られる。記録件数の比を南極全体のペンギンの種類の比率と解釈しない。島とペンギンの種類の違いを単純に分離できる調査設計でもない。

くちばしの測定にはノギス，翼には定規，体重にはばねばかりが使われた。詳しい対象の選び方と測定方法は原著の Materials and Methods を参照する。本教材では，収録された測定値の分布・対応関係を記述する。母集団の代表性，因果関係，未観測の個体の分類性能を，図の見た目だけで保証しない。

## 原著と原データ

Gorman, K. B., Williams, T. D., & Fraser, W. R. (2014). *Ecological Sexual Dimorphism and Environmental Variability within a Community of Antarctic Penguins (Genus Pygoscelis).* PLOS ONE, 9(3), e90081. [原著](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0090081)。

配布元の [原データ案内](https://allisonhorst.github.io/palmerpenguins/articles/download.html) に，Environmental Data Initiative のそれぞれのペンギンの種類のデータが示されている。今回収録したものは，これらの原データそのものではなく，配布元で 8 列に整理された版である。

- Adelie：[原データ DOI](https://doi.org/10.6073/pasta/abc50eed9138b75f54eaada0841b9b86)
- Gentoo：[原データ DOI](https://doi.org/10.6073/pasta/2b1cff60f81640f182433d23e68541ce)
- Chinstrap：[原データ DOI](https://doi.org/10.6073/pasta/409c808f8fc9899d02401bdb04580af7)
