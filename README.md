# Machine Learning for Coral Reef Bleaching Recovery

Undergraduate research project (NIBM): can machine learning predict how coral reefs recover after mass bleaching, and which ecological and environmental conditions matter most?

**Status:** pilot complete; data audit found a provenance problem (below); the study is being re-scoped to a larger global dataset.

## Pilot study: Seychelles reefs

- **Data source:** 12 Seychelles fringing reefs from Robinson, Wilson & Graham (2019), *Coral Reefs* 38: 1255–1265, using model outputs published in the authors' repository [jpwrobinson/coral-bleach-recovery](https://github.com/jpwrobinson/coral-bleach-recovery).
- **Target:** years for coral cover to return to its 1994 pre-bleaching baseline after the 1998 mass bleaching.
- **Methods:** exploratory data analysis, then linear, polynomial and support vector regression, evaluated with leave-one-out cross-validation (n = 12).

## Data audit: what I found

The pilot models performed poorly, so I traced every column back to the original repository:

1. **The predictor columns in `exports/ml_base.csv` are not field measurements.** Their values match the authors' `posterior_sims.Rdata`: model-predicted recovery years along a simulated grid of each covariate. This also explains the apparent "missing" nutrient values (that simulation has only three points).
2. **The genuine target is `exports/base_sites_recovery.csv`**, from the authors' recovery-trajectory model. Its years are counted from 2004; adding 6 gives years since the 1998 bleaching, as in the authors' scripts.
3. **Six of the 12 recovery times are projections** beyond the last survey in 2014, not observed recoveries.
4. The real site-level predictors (depth, wave exposure, herbivore biomass, juvenile coral density, nitrogen, structural complexity) were not published; the authors provide them on request.

**Consequence:** the regression results from the pilot (best LOOCV R² ≈ 0.39, linear SVR) do **not** describe ecological relationships and should not be interpreted as findings. They are kept only as a record of the pilot.

## Next steps

1. Request the original site predictors from the authors to redo the Seychelles analysis correctly.
2. Re-scope to the **HeatCRD** global database (van Woesik & Kratochwill, 2024): 29,205 coral cover surveys at 12,266 sites in 83 countries, 1977–2020, with depth, exposure, turbidity, macroalgae, protection and heat-stress data.
3. Derive recovery events from repeat surveys and predict **recovery rate**, following Walker, Kratochwill & van Woesik (2024).
4. Evaluate with spatially grouped cross-validation (sites and regions held out together) to avoid leakage.

## Repository contents

```
exports/     pilot data files, model outputs and notebooks (Untitled.ipynb, ml.ipynb)
figures/     figures from the original authors' repository (Robinson et al. 2019)
litrature/   literature and dataset notes
```

## References

- Robinson, J.P.W., Wilson, S.K. & Graham, N.A.J. (2019). Abiotic and biotic controls on coral recovery 16 years after mass bleaching. *Coral Reefs* 38, 1255–1265. https://doi.org/10.1007/s00338-019-01831-7
- Graham, N.A.J. et al. (2015). Predicting climate-driven regime shifts versus rebound potential in coral reefs. *Nature* 518, 94–97.
- van Woesik, R. & Kratochwill, C. (2024). A global database on coral recovery following marine heatwaves. *Scientific Data* 11. Data: https://www.bco-dmo.org/dataset/933334
- Walker, A.S., Kratochwill, C.A. & van Woesik, R. (2024). Past disturbances and local conditions influence the recovery rates of coral reefs. *Global Change Biology* 30, e17112.
