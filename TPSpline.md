確認しました。**`tps_maps/`も平滑化係数を自動選択しています。FITはSciPy、設定の選定はプロジェクト側の実装です。**

| 項目 | `tps_maps/`の実装 |
|---|---|
| FITライブラリ | SciPy `RBFInterpolator` の薄板スプライン |
| 自動選択する値 | 平滑化係数 `smoothing` |
| 既定の候補 | `0, 1e-6, 1e-5, 1e-4, 1e-3, 1e-2, 0.1, 1` |
| 評価方法 | 既定5分割の交差検証 |
| 選定基準 | FITに使わなかった点の予測RMSEが最小。同値なら大きい係数 |
| d/qの扱い | 成分ごとに別々に選定 |

元の点とその鏡像点は同じ分割へ入れ、評価点の鏡像が学習側へ混入しないようにしています。選定後は全点で再FITします。[選定処理](/Users/sugusokothx/newspace/motor-branch/number-8-QW2P/calibration_tools/jp_model/tps_maps/core.py:140)

前の回答で説明したLegacyと異なり、**今回は基本曲線を先にFITする処理がなく、磁束そのものを直接TPSでFITしています。** 自動選択が見るのは未使用点への予測誤差であり、全点FIT後の学習残差ではありません。

高密度JMAGデータは不要ですが、**相反性拘束は入っていません。** 自動選定結果は `cv_scores.csv`、採用係数は `fit_summary.json` に保存されます。
