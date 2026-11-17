# 運航記録の出典

米国運輸統計局（BTS）の Reporting Carrier On-Time Performance，2024 年 1 月。月別の公開原表から `Origin` が JFK，EWR，LGA の行を抽出した。32,347 行，105 の予定到着空港を収録。1 行は BTS の収録対象となる国内線の予定便。欠航・到着先変更・欠損がある行も残している。春学期第 3 回の試作と同じ抽出データを使用した。 [BTS の月別原表](https://www.transtats.bts.gov/PREZIP/) ・ [使用した ZIP](https://www.transtats.bts.gov/PREZIP/On_Time_Reporting_Carrier_On_Time_Performance_1987_present_2024_1.zip)

使用した列は `FlightDate`，`Origin`，`Dest`，`Reporting_Airline`，`CRSDepTime`，`ArrDelay`，`Cancelled`，`Diverted`。CSV ではそれぞれ `date`，`origin`，`destination`，`carrier`，`scheduled_departure`，`arrival_delay`，`cancelled`，`diverted` に改名している。付属の原表の説明は ZIP 内の `readme.html` にある。