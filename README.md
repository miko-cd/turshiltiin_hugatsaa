```mermaid
flowchart TD
  S[Start]
  S --> I[Input df_all age_col gender_col cv_sample_max do_cv run_age_sweep]

  I --> A1[Age prep to numeric known_age valid]
  A1 --> D1{known_age empty}
  D1 -->|Yes| E1[Raise ValueError No known ages]
  D1 -->|No| A2[Compute 6-bin edges and labels]
  A2 --> A3[Map to age_bin6_true]
  A3 --> G1[Normalize gender to gender_norm]

  G1 --> F1[Base features build_feature_sets_base]
  F1 --> F2[Add derived numeric features]
  F2 --> F3[Add package features if present]
  F3 --> F4[Assemble task features]

  %% AGE branch
  F4 --> AG1[age_train_idx notna]
  AG1 --> AG2[X_age and y_age_str]
  AG2 --> AG3[LabelEncode to y_age_enc]
  AG3 --> AG4[Class weights sw_age]
  AG4 --> AG5[Preprocess ColumnTransformer]
  AG5 --> AG6{do_cv}
  AG6 -->|Yes| AG7[Stratified sample]
  AG7 --> AG8[CV 5fold metrics age]
  AG6 -->|No| AG9[Skip CV]
  AG8 --> AG10{run_age_sweep and XGB}
  AG9 --> AG10
  AG10 -->|Yes| AG11[Run sweep best params]
  AG10 -->|No| AG12[Use default params]
  AG11 --> AG13[Get classifier override]
  AG12 --> AG13[Get classifier]
  AG13 --> AG14[Fit final age pipeline]
  AG14 --> AG15{Missing ages}
  AG15 -->|Yes| AG16[Predict age_bin6_pred proba_max]
  AG15 -->|No| AG17[Set pred proba NaN]
  AG16 --> AG18[age_bin6_filled true else pred]
  AG17 --> AG18

  %% GENDER branch
  F4 --> GG1[g_known_idx M or F]
  GG1 --> GG2[X_g and y_g_str]
  GG2 --> GG3[LabelEncode to y_g_enc]
  GG3 --> GG4[Class weights sw_g]
  GG4 --> GG5[Preprocess ColumnTransformer]
  GG5 --> GG6{do_cv and two classes}
  GG6 -->|Yes| GG7[Stratified sample]
  GG7 --> GG8[CV 5fold metrics gender]
  GG6 -->|No| GG9[Skip CV]
  GG8 --> GG10[Get classifier]
  GG9 --> GG10
  GG10 --> GG11[Fit final gender pipeline]
  GG11 --> GG12{Missing gender}
  GG12 -->|Yes| GG13[Predict gender_pred proba_F]
  GG12 -->|No| GG14[Set pred proba NaN]
  GG13 --> GG15[gender_filled norm else pred]
  GG14 --> GG15

  AG18 --> O1[Set attrs age_bin6_edges labels]
  GG15 --> O1
  O1 --> O2[Return df metrics label_info]
  O2 --> END[End]
  ```
