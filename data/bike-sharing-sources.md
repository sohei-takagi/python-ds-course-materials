# Capital Bikeshare 貸出集計の出典

ワシントン D.C. 地域の Capital Bikeshare の 2011〜2012 年の実際の貸出記録を Hadi Fanaee-T が時間別・日別に集計したデータである。集計者は気象と暦の情報を付加している。元の貸出記録，気象情報，祝日の出典は公式 ZIP の `Readme.txt` にある。 [UCI Machine Learning Repository のデータ説明](https://archive.ics.uci.edu/dataset/275/bike+sharing+dataset)

## 配布元と加工

- Fanaee-T, H. (2013). *Bike Sharing* [Dataset]. UCI Machine Learning Repository. [データ DOI](https://doi.org/10.24432/C5W894) ・ [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
- Fanaee-T, H., & Gama, J. (2014). Event labeling combining ensemble detectors and background knowledge. *Progress in Artificial Intelligence*, 2, 113–127. 2013 年オンライン公開。 [原著 DOI](https://doi.org/10.1007/s13748-013-0040-3) ・ [研究機関の公開 PDF](https://repositorio.inesctec.pt/server/api/core/bitstreams/a3c1c933-3432-4934-9bd3-b2becf6ec6d7/content)
- [取得した UCI 公式 ZIP](https://archive.ics.uci.edu/static/public/275/bike+sharing+dataset.zip)。確認日 2026-11-17。ZIP の SHA-256 は `b70182d0d0508e9abbb79306ce5c0cec34869000f8220175ac83d11dbe845401`。
- `bike-sharing-hourly.csv`（原名 `hour.csv`）と `bike-sharing-daily.csv`（原名 `day.csv`）を収録。改名と改行コードの LF への変更を行った。列名，値，行順は配布元のまま。SHA-256 はそれぞれ `b03a2d02e8c10f435c43c7f0b358b7e34a003afea53dbc37f0183f2763295133` と `537e98e2c8b8f53e3094d953f847788b1dc224764a4a1e538b3e1ec4e30dac8a`（LF 版）。
- 現在の事業者配布の利用ログには別の利用条件がある。本書で配布するのは UCI の集計版。 [Capital Bikeshare の原ログ案内](https://capitalbikeshare.com/system-data) ・ [原ログの利用条件](https://capitalbikeshare.com/data-license-agreement)

## 行と主な列

時間別表の 1 行は一つの日付・時刻帯に集計された貸出件数と付帯情報。日別表の 1 行は一つの日付に集計された値。期間は 2011-01-01〜2012-12-31。時間別 17,379 行（公式 ZIP 付属説明と実ファイルで確認），日別 731 行。

| 列 | 意味 |
| --- | --- |
| `dteday` | 日付。 |
| `hr` | 配布元の時計時刻，0〜23。たとえば 17 は 17 時台。時間別表のみ。 |
| `cnt` | 貸出件数。`casual + registered`。人を重複なく数えた人数や，その時刻に貸出中の自転車数とは区別する。 |
| `casual`, `registered` | 未登録区分，登録区分に集計された貸出件数。 |
| `weekday` | 曜日。日曜 0，月曜 1，…，土曜 6。実際の暦日と照合した。 |
| `holiday` | 配布元が参照した D.C. 行政機関の休日表による印。`1` が祝日。 |
| `workingday` | 土日・祝日以外が 1，土日・祝日が 0。雇用，通勤目的，自転車の営業状態を直接測った列とは異なる。 |
| `windspeed` | 配布元で正規化された風速指標。教材では原値のまま用いる。 |
| `temp`, `atemp` | 配布元で正規化された気温・体感温度指標。公式紹介ページと付属説明で換算式が一致しないため，教材では原値のまま用いる。 |
| `weathersit` | 配布元による気象カテゴリ。時間帯別の具体的な降水量は含まない。 |

祝日の例は 2012-07-04（水）の独立記念日と 2012-04-16（月）の D.C. Emancipation Day。祝日表の範囲は米国連邦職員の休日表や日本の祝日と異なる。 [ワシントン D.C. 人事局の 2012 年祝日表](https://dchr.dc.gov/sites/default/files/dc/sites/dchr/publication/attachments/DCHR_edpm_12_261_schedules_holidays_2012_2013.pdf)

## 時刻と行の欠落

時間別・日別表の収録行の中に空欄はなかった。毎日 0〜23 時の時計表示を並べた 17,544 通りのうち，時間別表に行があるのは 17,379 通り。日別表には全 731 日の行があり，時間別表の収録行を日別に `sum()` すると全日で `cnt`，`casual`，`registered` が日別表と一致する。この照合は集計同士の整合を確かめるもので，個々の欠けた時刻に実際の貸出が 0 件だったことまでは確かめられない。

時刻列に UTC オフセットはない。事業者の `Start date` を同じ時計表示で集計すると UCI の時間別の形・空白の多くが一致し，夏時間開始日の 2 時台は両方のデータに行がない。ワシントン D.C. 地域の時計表示と整合するが，資料にタイムゾーンや夏時間終了時の繰り返し 1 時台の集計規則の明記は確認できなかった。本書では配布元の時計表示として扱う。 [米国の夏時間規則（NIST）](https://www.nist.gov/pml/time-and-frequency-division/popular-links/daylight-saving-time-dst)
