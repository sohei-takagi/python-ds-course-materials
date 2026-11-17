# コンクリートの圧縮強度データ

## 出典と利用条件

I-Cheng Yeh が UCI Machine Learning Repository で公開した **Concrete Compressive Strength** を使用する。原表は `Concrete_Data.xls`，説明は同梱の `Concrete_Readme.txt`。取得日：2026-11-17。

- [UCI の配布ページ](https://archive.ics.uci.edu/dataset/165/concrete+compressive+strength)
- [公式 ZIP](https://archive.ics.uci.edu/static/public/165/concrete%2Bcompressive%2Bstrength.zip)
- Yeh, I. (1998). *Concrete Compressive Strength* [Dataset]. UCI Machine Learning Repository. [doi:10.24432/C5PK67](https://doi.org/10.24432/C5PK67).
- 原著：I-Cheng Yeh (1998). “Modeling of strength of high-performance concrete using artificial neural networks.” *Cement and Concrete Research*, 28(12), 1797–1808. [doi:10.1016/S0008-8846(98)00165-3](https://doi.org/10.1016/S0008-8846(98)00165-3).

UCI の利用条件は [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)。元データの著作権表示：**Copyright Prof. I-Cheng Yeh**。原表の説明に従い，上の著作権表示と論文情報を保持する。教材では列名の短縮，CSV への変換，原行番号・配合の識別番号・授業用分割の追加を行った。原著の本文は今回取得できておらず，研究の詳細は配布元の説明から確認できる範囲にとどめる。

## 一行と各列の意味

一行は，ある配合・材齢で測った圧縮強度の記録。配布元によると強度は実験室で測定された値である。配合の材料はコンクリート 1 m³ 当たりの質量，材齢は日数，圧縮強度は MPa。特定の建物の安全性や，現場の施工条件まで記録したデータではない。

| 教材の列 | 原表の項目 | 単位・役割 |
|---|---|---|
| `cement` | Cement | セメント，kg/m³ |
| `slag` | Blast Furnace Slag | 高炉スラグ，kg/m³ |
| `fly_ash` | Fly Ash | フライアッシュ，kg/m³ |
| `water` | Water | 水，kg/m³ |
| `superplasticizer` | Superplasticizer | 高性能減水剤，kg/m³ |
| `coarse_aggregate` | Coarse Aggregate | 粗骨材，kg/m³ |
| `fine_aggregate` | Fine Aggregate | 細骨材，kg/m³ |
| `age` | Age | 材齢，日 |
| `strength` | Concrete compressive strength | 圧縮強度，MPa。予測対象。 |
| `record_id` | 教材で追加 | 原表のデータ行を 1 から数えた番号。試験体固有の ID ではない。 |
| `recipe_id` | 教材で追加 | 7 材料の量が完全に一致する行をまとめた番号。原表での初出順。 |
| `split` | 教材で追加 | `train`，`validation`，`holdout`。モデルの入力には使わない。 |

数値は原表に保存された精度を保持し，欠損補完・標準化・丸め・行の削除はしていない。表示を小数第 2 位などへ丸める場合も，計算には元の値を使う。材齢は 1〜365 日。欠損は 0。原 9 列がすべて同じ行は後出の 25 行，入力 8 列が同じ行は後出の 34 行にある。重複が再測定・複製などのどれに当たるかを識別する情報がないため，勝手に削除せず残した。これらを独立した実験だと断定しない。

## 授業用の分割

第 20〜22 章では，**配合と試験する材齢が指定された段階で，学習に含まれなかった配合の圧縮強度を予測する**。材齢は試験後に初めて分かる値として使うのでなく，予測したい試験時点の指定である。同じ配合で別の材齢を測った行を学習と検証へ分けると，初めての配合への予測とは異なる課題になる。

そこで 7 材料の完全一致で 428 群を作り，群を単位に分割した。第 1 段階は `GroupShuffleSplit(test_size=0.2, random_state=2026)` で保留群を取り分け，第 2 段階は残る群に `test_size=0.25, random_state=2027` を適用して検証群を取り分けた。いずれも `n_splits=1`。分割は強度を参照せずに固定し，成績がよくなるまで乱数を選び直していない。割合は行数でなく群数に対する指定である。

| 分割 | 行数 | 配合群数 |
|---|---:|---:|
| 学習 | 631 | 256 |
| 検証 | 208 | 86 |
| 保留 | 191 | 86 |

同一配合の記録は一つの分割にしか入らない。ただし，配合が近い行や実験の出所の依存までは識別できない。研究機関・施工現場・新しい年代への汎化を直接検証したとは言えない。公開 CSV には保留の値もあるが，授業では候補比較に使わず，評価前にモデル・手順を固定する。第 20〜22 章の比較に保留結果を使わない。

## 再作成

公式 ZIP の SHA-256 は `dad85d14de8aee4e07479daa774e6b569a313715b71a3b92c95a07cf91c2c9a7`。再作成は `scripts/prepare_concrete_data.py` で行う。変換用環境は pandas 3.0.3，scikit-learn 1.8.0，xlrd 2.0.2。学生には xlrd は不要。

```powershell
python scripts/prepare_concrete_data.py <取得した公式ZIP>
```

原データの列順を上の 9 列に対応させ，群と分割を作り，`data/concrete-strength.csv` を出力する。教材の計算はこの CSV から実行する。数値の小数桁を表示用に丸めた CSV へ差し替えると，完全一致による群分けが変わる可能性があるため，原表から再作成する。
