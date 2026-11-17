# 米粒の形状と品種のデータ

## 出典と利用条件

Ilkay Cinar と Murat Koklu が UCI Machine Learning Repository で公開した **Rice (Cammeo and Osmancik)** を使用する。取得日 2026-11-17。配布アーカイブの `Rice_Cammeo_Osmancik.arff` が元の数値表。

- Cinar, I., & Koklu, M. (2019). *Rice (Cammeo and Osmancik)* [Dataset]. UCI Machine Learning Repository. [配布ページ](https://archive.ics.uci.edu/dataset/545/rice+cammeo+and+osmancik) ・ [データ DOI](https://doi.org/10.24432/C5MW4Z) ・ [公式 ZIP](https://archive.ics.uci.edu/static/public/545/rice+cammeo+and+osmancik.zip)
- Cinar, I., & Koklu, M. (2019). Classification of Rice Varieties Using Artificial Intelligence Methods. *International Journal of Intelligent Systems and Applications in Engineering*, 7(3), 188–194. [原著 DOI](https://doi.org/10.18201/ijisae.2019355381) ・ [出版社の公開 PDF](https://ijisae.org/index.php/IJISAE/article/download/1068/599)

UCI のデータ表のライセンスは [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)。教材では ARFF を CSV に変え，列名を短縮し，元の行番号と授業用の分割を追加した。論文の写真や装置図は教材データに含めていない。

## 一行と各列の意味

原著の第 2.1〜2.4 節によると，研究者は Cammeo と Osmancik を 50 g ずつ量った二つの試料を用意し，外光を遮った照明箱と黒い背景を使って撮影した。画像処理を経て，両試料から得た計 3,810 粒について七つの形状の値を求めた。一行は一粒の形状値と品種を表す。数値の元になる画像尺度から，実物の寸法を mm などへ換算する情報は，配布表に含まれていない。 [原著の画像取得と形状計測](https://ijisae.org/index.php/IJISAE/article/download/1068/599)

| 教材の列 | 元の列 | 意味 |
|---|---|---|
| `area` | `Area` | 粒の輪郭内の画素数。 |
| `perimeter` | `Perimeter` | 粒の輪郭に沿った周囲の長さ。画素尺度。 |
| `major_axis` | `Major_Axis_Length` | 粒の形を表す長い方の軸の長さ。 |
| `minor_axis` | `Minor_Axis_Length` | 粒の形を表す短い方の軸の長さ。 |
| `eccentricity` | `Eccentricity` | 粒の形を要約する楕円の離心率。細長さを示す。 |
| `convex_area` | `Convex_Area` | 粒を包む最小の凸な領域の画素数。 |
| `extent` | `Extent` | 粒を囲む外接矩形の面積に対する，輪郭内の面積の割合。 |
| `variety` | `Class` | 品種。元のラベル `Cammeo`・`Osmancik` を保持する。 |
| `record_id` | 教材で追加 | ARFF のデータ部分の先頭を 1 とした行番号。 |
| `split` | 教材で追加 | 授業用の区分 `train`・`validation`・`holdout`。 |

入力七列と品種ラベルの欠損は 0，この八列がすべて一致する重複行は 0。CSV は元行順で，最初の 1,630 行が Cammeo，続く 2,180 行が Osmancik。元行番号は収録行を照合するための管理情報で，品種ごとにまとまった元の並びを反映する。予測の入力には七つの形状値を使い，`record_id` と `split` は除く。分類の正解は元の品種名から `variety.eq("Cammeo")` で作り，Cammeo を正例とする。 [UCI の列の定義](https://archive.ics.uci.edu/dataset/545/rice+cammeo+and+osmancik)

## 授業用の分割

第 23〜25 章では，撮影して得た形状値から，この表の二品種のどちらかを識別する。区分をまたいで比べるのは，同じ撮影・測定の資料から取り分けた別の粒の記録である。

品種の割合をそろえるため，行を単位に二段階の層化無作為分割を行った。元行の 0 始まりの位置を `np.arange(3810)` で用意する。第 1 段階は `train_test_split(test_size=0.2, stratify=cammeo, random_state=2026)` で保留を分ける。第 2 段階は残った位置を返された順序のまま使い，その位置の品種で層化して `test_size=0.25, random_state=2027` で検証と学習に分ける。層化用の `cammeo` は Cammeo を 1，Osmancik を 0 とした配列。区分を元の位置へ書き戻してから CSV にする。

| 区分 | 行数 | Cammeo | Osmancik |
|---|---:|---:|---:|
| 学習 `train` | 2,286 | 978 | 1,308 |
| 検証 `validation` | 762 | 326 | 436 |
| 保留 `holdout` | 762 | 326 | 436 |
| 合計 | 3,810 | 1,630 | 2,180 |

検証行は三章で同じものを使い，モデル・閾値・誤分類の違いを比べる。保留行はこれらの選択から外す。保留の内訳は，分割時に品種の割合をそろえた結果である。

公開表には収穫ロット，収穫日や撮影日，装置，粒そのものの識別子がない。元行番号は，どのロットや撮影の行かを示すものではない。学習・検証・保留の行を分けても，互いに異なるロットや撮影条件を割り当てた評価にはならない。別の収穫ロットや装置に使う場合は，それらを区別して記録した新しい資料で確かめる必要がある。

## 再作成

公式 ZIP から `Rice_Cammeo_Osmancik.arff` を取り出す。リポジトリ直下に展開した場合は，そのディレクトリから次を実行する。別の場所へ展開した場合は，最後の引数を ARFF のパスに替える。教材の実行時は生成済み CSV を使う。

```powershell
python scripts/prepare_rice_data.py Rice_Cammeo_Osmancik.arff
```

分割と変換の検算には Python 3.13，NumPy 2.4.6，scikit-learn 1.8.0 を使用した。`data/rice-grains.csv` は UTF-8・LF で，元表の数値の表記を丸めず移している。別出力への再生成はバイト単位で一致し，元の ARFF の値と品種，固定した分割の所属を全行で照合した。

| ファイル | SHA-256 |
|---|---|
| 調査時に取得した UCI 公式 ZIP | `fe94e42046b829de21b92b0ffb6a22774fde021328cae799faa802a14e8dbed9` |
| ZIP から展開した ARFF | `1af97883100c89de2ea2972f7a28d428f4f1c14711a61defc0b0569e9eb65665` |
| 教材 CSV | `f6d8aa95dce495a58e56d9624319ec1e2826aa2a554347e4afa7beac091ec5b3` |
