flowchart TD
```mermaid
  S([Start]) --> I[Input: df_all, age_col, gender_col, cv_sample_max, do_cv, run_age_sweep]
  I --> A1[Age prep: c_age -> numeric; known_age = notna & != -1]
  A1 --> D1{known_age empty?}
  D1 -- Yes --> E1([Raise ValueError: No known ages])
  D1 -- No --> A2[Compute 6-bin edges/labels -> make_age_bin6_mapper]
  A2 --> A3[Map age_bin6_true]
  A3 --> G1[Normalize gender -> gender_norm]
  G1 --> F1[Base features: _build_feature_sets_base(df)]
  F1 --> F2[Derived numeric ratios/rates: _add_derived_numeric]
  F2 --> F3[Optional package feats: _maybe_add_package_features(c_package)]
  F3 --> F4[Assemble per-task features:
            categorical_age = categorical_all + gender_norm
            categorical_gender = categorical_all (no c_gender, gender_norm)
            numeric_all = numeric_base + derived + pkg_num]

  F4 --> subgraph AGE[Age model (multiclass, 6-bin)]
     direction TB
     AG1[age_train_idx = age_bin6_true notna]
     AG2[X_age = features; y_age_str = labels]
     AG3[LabelEncoder -> y_age_enc (0..K-1)]
     AG4[Class weights sw_age (inv. freq)]
     AG5[pre_age = ColumnTransformer(OHE+Impute)]
     AG6{do_cv?}
     AG6 -- Yes --> AG7[_stratified_sample(X_age, y_age_enc, cv_sample_max)]
     AG7 --> AG8[Evaluate CV (5-fold) -> age_metrics]
     AG8 --> AG9{run_age_sweep AND _HAS_XGB?}
     AG6 -- No --> AG9
     AG9 -- Yes --> AG10[_run_age_sweep(pre_age, X_age, y_age_enc, sw_age)]
     AG9 -- No --> AG11[Skip sweep]
     AG10 --> AG12[Get classifier: _get_classifier('age') + override best params]
     AG11 --> AG12[Get classifier: _get_classifier('age')]
     AG12 --> AG13[Fit final age pipeline with sw_age]
     AG13 --> AG14{Missing ages? (isna or == -1)}
     AG14 -- Yes --> AG15[Predict age_bin6_pred; proba_max if available]
     AG14 -- No --> AG16[Set pred/proba NaN]
     AG15 --> AG17[age_bin6_filled = true else pred]
     AG16 --> AG17
  end

  F4 --> subgraph GENDER[Gender model (binary)]
     direction TB
     GG1[g_known_idx = gender_norm in {M,F}]
     GG2[X_g = features; y_g_str]
     GG3[LabelEncoder -> y_g_enc]
     GG4[Class weights sw_g (inv. freq)]
     GG5[pre_gender = ColumnTransformer(OHE+Impute)]
     GG6{do_cv AND two classes?}
     GG6 -- Yes --> GG7[_stratified_sample(X_g, y_g_enc, cv_sample_max)]
     GG7 --> GG8[Evaluate CV (5-fold) -> gender_metrics]
     GG6 -- No --> GG9[Skip CV]
     GG8 --> GG10[Get classifier: _get_classifier('gender')]
     GG9 --> GG10
     GG10 --> GG11[Fit final gender pipeline with sw_g]
     GG11 --> GG12{Missing gender? (not in {M,F})}
     GG12 -- Yes --> GG13[Predict gender_pred; proba for 'F' if available]
     GG12 -- No --> GG14[Set pred/proba NaN]
     GG13 --> GG15[gender_filled = norm else pred]
     GG14 --> GG15
  end

  AGE --> O1[Attach attrs: age_bin6_edges, age_bin6_labels]
  GENDER --> O1
  O1 --> O2[Return:
             df (+ new cols),
             metrics = {age, gender},
             label_info = {labels, edges}]
  O2 --> END([End])
  ```
