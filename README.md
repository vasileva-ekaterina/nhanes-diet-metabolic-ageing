# Diet Quality, Metabolic Risk and Survival in Adults 50+ (NHANES 2011–2012)

## Summary

## Why this question

## Data and population

### Population

The analysis uses the 2011–2012 cycle of the National Health and Nutrition Examination Survey (NHANES), run by the US National Center for Health Statistics (NCHS), restricted to participants aged 50 and over. This cycle was chosen because NCHS links its participants to death records through the end of 2019 in a public-use mortality file, giving roughly eight years of follow-up. More recent cycles have too little follow-up for a survival analysis.

Diet is measured by the first-day 24-hour dietary recall. Participants are kept only if NHANES flags their recall as reliable (`DR1DRSTZ = 1`), and then only if their reported energy intake is plausible by the Willett cut-offs: 500–3,500 kcal for women, 800–4,000 kcal for men. The energy step removes 100 participants, 4.3% of those with a reliable recall.

### Sample flow

```
NHANES 2011–2012 participants                          9,756
  → aged 50 and over                                   2,704
  → record in the dietary file                         2,566
  → reliable recall (DR1DRSTZ = 1)                     2,306
  → plausible energy intake                            2,206   analytic sample
      │
      ├─ fasting glucose and triglycerides measured    1,015
      │    └─ complete covariates                        906   metabolic syndrome models,
      │                                                        prediction benchmark
      │
      └─ linked to mortality records                   2,202   440 deaths
           └─ complete covariates                      1,998   398 deaths, Cox model
```

The analytic sample of 2,206 divides according to what each analysis needs. The metabolic syndrome outcome requires fasting glucose and triglycerides, which 1,015 participants have; 906 of them have complete data on the model covariates, and the prediction benchmark uses the same 906. The survival analysis has no fasting requirement: it uses all 2,202 participants NCHS could link to mortality records (4 could not be linked for insufficient identifying data), of whom 440 (20.0%) died during follow-up. Among those still alive at the end of follow-up, the median follow-up is 96 months. By Kaplan–Meier estimate, 96.5% of the sample were alive at 2 years, 89.1% at 5 years and 79.5% at 8 years. The Cox model's complete cases number 1,998, with 398 deaths.

Almost all the loss at the complete-case steps is from missing household income: 104 of the 109 participants dropped from the metabolic syndrome models, and 198 of the 204 dropped from the Cox model. Methods describes how those dropped compared with those kept.

### Data files

The data are not redistributed in this repository. They are free to download from NCHS, and their use is governed by the [NCHS Data User Agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html), which a user accepts on download. The notebook reads the twelve files below from a folder named `data/` at the top level of the project.

| File | Contents | SHA-256 |
|---|---|---|
| [`BMX_G.xpt`](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Examination&Cycle=2011-2012) | Body measures | `4814bfc3047ed400b9d43d285f8c3ea7c940ac6489404a9b699579715d158ec3` |
| [`BPQ_G.xpt`](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Questionnaire&Cycle=2011-2012) | Blood pressure and cholesterol medication questions | `39c5a0ee1d76e6f3df867c0057986a21f4a028038002c43214c5af8bdec83cae` |
| [`BPX_G.xpt`](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Examination&Cycle=2011-2012) | Blood pressure readings | `1dd66110f41465d601cd54f1e3c6010baafb9375f09fbcaa3aef2536876146fd` |
| [`DEMO_G.xpt`](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Demographics&Cycle=2011-2012) | Demographics | `eaf0525d1952626885af3e935415a1f66ad62c18698080e7354789c125af252d` |
| [`DIQ_G.xpt`](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Questionnaire&Cycle=2011-2012) | Diabetes questions | `a9d5475e0cd66d6a7bc30230345ed0172a9abcbc0a7a4145a35025c827a52a87` |
| [`DR1TOT_G.xpt`](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Dietary&Cycle=2011-2012) | First-day total nutrient intakes | `d5589de4cd987eef86dd13b3b2bc3db9186ab80a87c96a3968421ddef9235c57` |
| [`GHB_G.xpt`](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Laboratory&CycleBeginYear=2011) | Glycohemoglobin (HbA1c) | `8ff7cc95461fdfd47d2c9640b944a2bcd45926a3c0e64c98a09a4cef6c0f88d7` |
| [`GLU_G.xpt`](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Laboratory&CycleBeginYear=2011) | Fasting glucose | `e9c3816dbdfc21cd53a23d63c3610d2550662ee41a21df0a5b72c4a16cd1a8fd` |
| [`HDL_G.xpt`](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Laboratory&CycleBeginYear=2011) | HDL cholesterol | `ffc060d394cc487f3206462fdfc77648bdbdd6e279bfc5dd8e9cbba3ccdcac4f` |
| [`SMQ_G.xpt`](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Questionnaire&Cycle=2011-2012) | Smoking questions | `69f2375f036a815082b1e97110e79c505b91e0985c61696f8eaacf94828115dc` |
| [`TRIGLY_G.xpt`](https://wwwn.cdc.gov/nchs/nhanes/search/datapage.aspx?Component=Laboratory&CycleBeginYear=2011) | Triglycerides | `4b7f9c73f5169f8bb582823d00a8e488d6985ea9d8784f85ee63ff60179589a0` |
| [`NHANES_2011_2012_MORT_2019_PUBLIC.dat`](https://ftp.cdc.gov/pub/Health_Statistics/NCHS/datalinkage/linked_mortality/) | Public-use linked mortality file (fixed-width text) | `ff98b6c87172643426b1c6d20f76495592bebd618ea18f742d98a44b84456b30` |

To check that downloaded files are identical to the ones this analysis used, run this in the folder that holds them (tested on macOS, zsh):

```
shasum -a 256 *.xpt *.dat
```

Each fingerprint in the output should match the table. The files are listed in the same order as the table.

### Mortality data

The mortality file comes from the NCHS public-use linked mortality files, which follow NHANES participants' vital status through the end of 2019. It is downloaded from the [NCHS linked mortality FTP directory](https://ftp.cdc.gov/pub/Health_Statistics/NCHS/datalinkage/linked_mortality/), which NCHS names as the authentic source of these files. Its fixed-width layout is described in the [NCHS data dictionary](https://www.cdc.gov/nchs/data/datalinkage/public-use-linked-mortality-files-data-dictionary.pdf).

The data are cited as NCHS asks:

> National Center for Health Statistics Division of Analysis and Epidemiology. Continuous NHANES Public-use Linked Mortality Files, 2019. Hyattsville, Maryland. Available from: `https://www.cdc.gov/nchs/data-linkage/mortality-public.htm`. doi:10.15620/cdc:117142.

The web page named in the citation now redirects to a general National Death Index page, so the FTP directory above is given as the download source and the DOI as the permanent identifier.

**Acknowledgement and disclaimer.** The linked mortality data are provided by the National Center for Health Statistics. The analyses, interpretations and conclusions in this repository are the author's alone; NCHS is responsible only for the initial data.

## Methods

The analysis is built in four layers, each completed before the next began:

1. **Diet quality score:** a nutrient-based score from the 24-hour dietary recall.
2. **Diet and metabolic syndrome:** cross-sectional logistic regression.
3. **Diet and mortality:** Kaplan–Meier curves and a Cox proportional hazards model over about eight years of follow-up.
4. **Prediction benchmark:** whether a flexible model predicts metabolic syndrome better than logistic regression.

### Diet quality score

The score follows the logic of DASH-style indices. Eight nutrients are scored: five that a healthy diet should be high in are rewarded (fibre, potassium, magnesium, calcium and vitamin C), and three it should be low in are penalised (sodium, saturated fat and total sugars). A nutrient-based index with published precedent was preferred to an invented one.

Each nutrient is first expressed as a density per 1,000 kcal, so that the score reflects what a diet is made of rather than how much was eaten. Densities are then divided into quintiles within the analytic sample and scored 1 to 5, reversed for the penalised nutrients so that a higher score always means a better diet. The eight component scores are summed with equal weight, giving a possible range of 8 to 40. The observed range is 10 to 37 (mean 24.0, SD 5.56).

Equal weighting was checked rather than assumed: no pair of components correlates above 0.8 (the highest, potassium with magnesium, is 0.71), so no component duplicates another.

Two candidate components were left out. Carotenoid intake on a single day depends heavily on whether a carotenoid-rich food happened to be eaten, so one recall ranks people's usual intake poorly. Protein does not distinguish diet quality without information on its food sources: 80 g from processed meat would score the same as 80 g from legumes.

Energy adjustment by density was chosen over the residual method (regressing each nutrient on total energy), which is equally standard but harder for a non-specialist to follow. A re-run with the residual method is deferred.

### Metabolic syndrome

Metabolic syndrome is defined by the harmonised criteria: a participant has it if they meet three or more of the five below. It is assessed on the 1,015 participants with fasting glucose and triglycerides.

| Criterion | Met if |
|---|---|
| Waist circumference | ≥ 102 cm (men), ≥ 88 cm (women) |
| Blood pressure | systolic ≥ 130 or diastolic ≥ 85 mmHg, or on blood pressure medication |
| Fasting glucose | ≥ 100 mg/dL, or diagnosed diabetes, or on diabetes medication |
| Triglycerides | ≥ 150 mg/dL, or on cholesterol medication |
| HDL cholesterol | < 40 mg/dL (men), < 50 mg/dL (women), or on cholesterol medication |

Medication is part of the definition because the harmonised criteria specify it, and leaving it out would bias this analysis in a predictable direction. People on treatment tend to have normal measurements and, having been advised to change their diet, better diets too. Counting them as unaffected would make good diets look more protective than they are.

Blood pressure is the mean of the second and third readings, since the first tends to be raised by the alerting response; where those are missing, whichever readings exist are used. Diagnosed diabetes means answering yes to the diabetes question (`DIQ010 = 1`); "borderline" does not count. Diabetes medication (insulin or oral agents) meets the glucose criterion even without a diagnosis, since the criteria refer to drug treatment, not diagnosis. In practice this matters little: in the fasting subsample, fasting glucose alone identifies 682 participants and the diagnosis and medication arms together add 6. Four "don't know" answers to medication questions are coded as not on medication.

Cholesterol medication needs more care. Following the harmonised wording, one "yes" to the cholesterol medication question satisfies both lipid criteria at once, so 380 of the 1,015 (37.4%) start with two criteria before any measurement is taken. The defence would be that low HDL and high triglycerides go together clinically, but in this sample they mostly do not: among the 635 participants not on cholesterol medication, only 72 (11.3%) meet both lipid criteria on their measurements. The question also asks about any cholesterol-lowering prescription, and in this age group most are likely statins, prescribed for LDL, which is not a metabolic syndrome component. The medicated definition is kept as primary because it follows the published criteria and standard practice in NHANES analyses. A measurements-only version of both lipid criteria is run alongside it as a pre-specified sensitivity analysis.

A four-criteria definition based on HbA1c, which needs no fasting sample and so could use all 2,206 participants, is deferred. It would be a substantive check rather than a formality, because the fasting subsample is not a random subset: morning attendance is related to employment, age and health.

## Results

## Limitations

## What I'd do next

## Reproducibility
