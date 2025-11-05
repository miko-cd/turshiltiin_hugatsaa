```mermaid
flowchart TD
  S([Start])
  S --> I[Input: df_all, age_col, gender_col, cv_sample_max, do_cv, run_age_sweep]

  I --> A1[Age prep: c_age -> numeric; known_age = notna & != -1]
  A1 --> D1{known_age empty?}
  D1 -->|Yes| E1([Raise ValueError: No known ages])
  D1 -->|No| A2[Compute 6-bin edges/labels]
  A2 --> A3[Map to age_bin6_true]
  A3 --> G1[Normalize gender -> gender_norm]

  G1 --> F1[Base features: _build_feature_sets_base]
  F1 --> F2[Derived numeric: _add_derived_numeric]
  F2 --> F3[Package feats: _maybe_add_package_features]
  F3 --> F4[Assemble per-task features]

  %% ----- AGE subgraph -----
  F4 --> AG0
  subgraph AGE [Age model (multiclass)]
    direction TB
    AG0 --> AG1[age_train_idx = age_bin6_true notna]
    AG1 --> AG2[X_age, y_age_str]
    AG2 --> AG3[LabelEncode -> y_age_enc]
    AG3 --> AG4[Class weights sw_age]
    AG4 --> AG5[Preprocess: ColumnTransformer]
    AG5 --> AG6{do_cv?}
    AG6 -->|Yes| AG7[_stratified_sample]
    AG7 --> AG8[CV (5-fold) -> age_metrics]
    AG8 --> AG9{run_age_sweep & _HAS_XGB?}
    AG6 -->|No| AG9
    AG9 -->|Yes| AG10[_run_age_sweep -> best params]
    AG9 -->|No| AG11[Skip sweep]
    AG10 --> AG12[Get classifier + override]
    AG11 --> AG12[Get classifier]
    AG12 --> AG13[Fit final age pipeline (sw_age)]
    AG13 --> AG14{Missing ages?}
    AG14 -->|Yes| AG15[Predict age_bin6_pred; proba_max]
    AG14 -->|No| AG16[Set pred/proba NaN]
    AG15 --> AG17[age_bin6_filled = true else pred]
    AG16 --> AG17
  end

  %% ----- GENDER subgraph -----
  F4 --> GG0
  subgraph GENDER [Gender model (binary)]
    direction TB
    GG0 --> GG1[g_known_idx = gender_norm in {M,F}]
    GG1 --> GG2[X_g, y_g_str]
    GG2 --> GG3[LabelEncode -> y_g_enc]
    GG3 --> GG4[Class weights sw_g]
    GG4 --> GG5[Preprocess: ColumnTransformer]
    GG5 --> GG6{do_cv & two classes?}
    GG6 -->|Yes| GG7[_stratified_sample]
    GG7 --> GG8[CV (5-fold) -> gender_metrics]
    GG6 -->|No| GG9[Skip CV]
    GG8 --> GG10[Get classifier]
    GG9 --> GG10
    GG10 --> GG11[Fit final gender pipeline (sw_g)]
    GG11 --> GG12{Missing gender?}
    GG12 -->|Yes| GG13[Predict gender_pred; proba(F)]
    GG12 -->|No| GG14[Set pred/proba NaN]
    GG13 --> GG15[gender_filled = norm else pred]
    GG14 --> GG15
  end

  AGE --> O1[Attach attrs: age_bin6_edges, age_bin6_labels]
  GENDER --> O1
  O1 --> O2[Return df (+new cols), metrics, label_info]
  O2 --> END([End])
  ```
