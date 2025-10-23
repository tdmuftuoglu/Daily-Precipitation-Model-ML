# NEWSTUDY: Daily Precipitation ML Benchmark (2001–2024)
## NEWSTUDY: Günlük Yağış İçin ML Kıyaslaması (2001–2024)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/tdmuftuoglu/Daily-Precipitation-Model-ML/blob/main/NEWSTUDY.ipynb)
![Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)
![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)

This repository contains a **bilingual, one-block Google Colab pipeline** for **daily precipitation forecasting** with **NASA POWER** satellite meteorology (2001–2024). It follows the paper’s design: (i) **trend & frequency** analysis (Mann–Kendall; Gumbel return levels with 95% CIs), (ii) **automated feature selection** from lagged predictors, and (iii) a **twelve-model bake-off** covering tree ensembles, neural nets, time-series models, stacking, and a hybrid Prophet+XGB. The champion in the study is **ANN** (**R² ≈ 0.680**), closely followed by **XGBoost (≈0.674)** and **Random Forest (≈0.665)**. The **100-year design rainfall** is ≈ **129.82 mm/day** (95% CI **[84.82, 146.83]**). Event-based verification shows **ANN** leading for **>20 mm/day** with **CSI ≈ 0.448**.

---
Bu depo, **NASA POWER** uydu meteorolojisi (2001–2024) ile **günlük yağış tahmini** için **çift dilli, tek blok Colab** hattı sunar. Makalenin kurgusunu izler: (i) **trend & frekans** analizi (Mann–Kendall; Gumbel ile %95 GA), (ii) gecikmeli özniteliklerden **otomatik özellik seçimi**, (iii) ağaç toplulukları, sinir ağları, zaman serisi modelleri, stacking ve hibrit Prophet+XGB’den oluşan **on iki model**lik bir kıyaslama. Çalışmada **şampiyon model ANN**’dir (**R² ≈ 0.680**); **XGBoost (≈0.674)** ve **Rastgele Orman (≈0.665)** çok yakındır. **100 yıllık tasarım yağışı** ≈ **129.82 mm/gün** (**%95 GA [84.82, 146.83]**). **>20 mm/gün** için **olay-tabanlı doğrulamada** **ANN**’in **CSI ≈ 0.448** ile önde olduğu gözlenir.

---

## Features / Özellikler

- **Comprehensive model suite / Kapsamlı model seti:** RF, XGBoost, LightGBM, **ANN**, **LSTM**, **GRU**, SARIMAX, Prophet, Stacking, **Hybrid Prophet+XGB**, + 2 baselines (t−1, t−365).  
  / RF, XGBoost, LightGBM, **ANN**, **LSTM**, **GRU**, SARIMAX, Prophet, Stacking, **Hibrid Prophet+XGB**, + 2 basit model (t−1, t−365).
- **Leakage-safe workflow / Sızıntısız iş akışı:** Kronolojik ayırma (**2001–01–01 → 2020–12–31** eğitim; **2021–01–01 → 2024–12–31** test), yalnızca geçmişe ait gecikmeler (t−1..t−3).  
  / Chronological split (train **2001–01–01 → 2020–12–31**; test **2021–01–01 → 2024–12–31**), lag-only features (t−1..t−3).
- **Trend & frequency / Trend & frekans:** Mann–Kendall eğilim; Gumbel ile **tasarım yağışları** ve **%95 GA**.  
- **Automated feature selection / Otomatik özellik seçimi:** XGBoost tabanlı önem sıralamasıyla **en belirgin 15 öznitelik** (örn. **RH2M_t−1**, **PS_t−1**, **ALLSKY_SFC_SW_DWN_t−2**).  
- **Event-based verification / Olay-tabanlı doğrulama:** Eşikler **P90 (≈4.85 mm/gün)** ve **20 mm/gün** → **HitRate, FAR, CSI**.  
- **Explainability / Açıklanabilirlik:** **SHAP** global özet & beeswarm görselleri.  
- **One-click reproducibility / Tek tık tekrarlanabilirlik:** Tüm tablolar ve şekiller uçtan uca yeniden üretilir; tüm çıktılar **ZIP** olarak indirilir.

---

## How to Run / Nasıl Çalıştırılır

The entire workflow is automated in Colab.  
Tüm iş akışı Colab’de otomatikleştirilmiştir.

1. **Open in Colab** → Click the badge above.  
   **Colab’de Aç** → Yukarıdaki rozete tıklayın.
2. **Upload data** → Select your `NASAPOWERDATASET.csv`.  
   **Veriyi yükle** → `NASAPOWERDATASET.csv` dosyanızı seçin.
3. **Runtime → Run all** → The notebook will:  
   **Çalışma Zamanı → Tümünü çalıştır** → Not defteri şunları yapar:
   - Build YEAR+DOY datetime index; clean & sort. / YEAR+DOY’dan zaman indeksi kurar; temizler ve sıralar.
   - Compute **Gumbel** return levels (**%95 CI**) & trend (**Mann–Kendall + Theil–Sen**).  
   - Create lags (t−1..t−3) for **target & predictors**. / Hedef ve girdiler için gecikmeler üretir.
   - Train **baselines** + **RF/XGB/LGBM/ANN/Prophet/SARIMAX/LSTM/GRU/Stacking/Hybrid**.  
   - Evaluate **RMSE, R²** and **event metrics** (P90, 20 mm).  
   - Generate **SHAP** plots & **concept diagrams** (RF/ANN/Stacking).  
   - Zip **all outputs** and offer download. / **Tüm çıktıları** ZIP’ler ve indirmeye sunar.

---

## Repository Contents / Repo İçeriği

- `NEWSTUDY.ipynb` → One-block Colab notebook (end-to-end). / Tek blok Colab not defteri (uçtan uca).
- `design_rainfall_table_with_CI.csv` → Gumbel return levels (+95% CI). / Gumbel tekerrür seviyeleri (+%95 GA).
- `GUMBEL_return_levels_CI.png` → Return period–level plot. / Tekerrür süresi–seviye grafiği.
- `FINAL_TREND_ANALYSIS_fixed.png` → MK + centered Theil–Sen trend. / MK + merkezli Theil–Sen trendi.
- `feature_importance_FIXED.png` → XGB feature importance. / XGB öznitelik önemleri.
- `ULTIMATE_FINAL_comparison_FIXED.csv` → Model comparison (RMSE, R²). / Model karşılaştırma (RMSE, R²).
- `model_performance_barchart_FIXED.png` → R² bar chart. / R² çubuk grafiği.
- `ULTIMATE_FINAL_comparison_FIXED.png` → Top-models time-series overlay. / En iyi modeller zaman serisi karşılaştırması.
- `champion_model_scatter_FIXED.png` → ANN parity scatter. / ANN saçılım grafiği.
- `EVENT_METRICS_top_models.csv` → Event metrics (P90, 20 mm). / Olay metrikleri (P90, 20 mm).
- `RF_SHAP_bar.png`, `RF_SHAP_beeswarm.png` → RF SHAP.
- `LGBM_SHAP_bar.png`, `LGBM_SHAP_beeswarm.png` → LGBM SHAP.
- `XGB_SHAP_bar.png`, `XGB_SHAP_beeswarm.png` → XGB SHAP.
- `random_forest_concept_FIXED.png` → RF concept diagram. / RF kavram şeması.
- `ann_architecture_FIXED.png` → ANN architecture. / ANN mimarisi.
- `stacking_architecture_FIXED.png` → Stacking architecture. / Stacking mimarisi.
- `NEWSTUDY_RESULTS_FIXED.zip` → All outputs bundled. / Tüm çıktılar paketli.

---

## Key Findings / Özet Bulgular

- **Champion model:** **ANN** with **R² ≈ 0.680** (RMSE ≈ 3.384 mm/gün).  
- **Runners-up:** **XGBoost ≈ 0.674**, **Random Forest ≈ 0.665**, **LightGBM ≈ 0.665**.  
- **Design rainfall:** **T=100 yıl → ~129.82 mm/gün** (**%95 GA [84.82, 146.83]**).  
- **Event skill:** **ANN**, **>20 mm/gün** için **CSI ≈ 0.448**; P90 eşiği ≈ **4.85 mm/gün**.  
- **Trend:** Yıllık maksimumlarda **istatistiksel olarak anlamlı bir trend yok** (MK).  

---

## Citation / Atıf

Please cite the paper and archive when using this repository:  
Bu repoyu kullanırken lütfen aşağıdakileri atıf verin:

- **Muftuoglu, T. D.** *A Comprehensive Comparative Analysis of ML Models for Daily Precipitation Forecasting Using Satellite-Based Meteorological Data (2001–2024).* OKU Journal of The Institute of Science and Technology (under review).  
- **Zenodo archive:** https://doi.org/10.5281/zenodo.17238381  
- **Code & project:** https://github.com/tdmuftuoglu/Daily-Precipitation-Model-ML

---

## License / Lisans

This project is licensed under the **MIT License**. See the [`LICENSE`](LICENSE) file for full terms.  
Bu proje **MIT Lisansı** ile dağıtılmaktadır. Tüm koşullar için [`LICENSE`](LICENSE) dosyasına bakın.
