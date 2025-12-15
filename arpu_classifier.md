``` mermaid
flowchart TD

    A[Start] --> B[Connect SQL Server]
    B --> C[Read APRU_increased → df_num]
    B --> D[Read categorical PREV<br/>→ df_categorical_prev]
    B --> E[Read categorical CUR<br/>→ df_categorical_cur]

    E --> F[Compute tenure<br/>today - c_initial_date → tenure_cur]
    D --> G[Merge prev/cur categorical<br/>on c_isdn → df_cat]
    F --> G

    G --> H[Build df_cat_clean<br/>age, gender, tenure,<br/>cur/prev package/payment/marketing/handset]
    H --> I[Create change flags<br/>change_handset, change_payment]

    C --> J[Join df_num + df_cat_clean<br/>on c_isdn → df_all]
    I --> J

    J --> K[Define label_increase<br/>delta_ARPU > 5000 → 1 else 0]
    K --> L[Handle age=-1 → 0,<br/>replace inf with NaN,<br/>drop NaN labels]

    L --> M[Select features<br/>num_cols + cat_cols + label_increase]
    M --> N[One-hot encode categoricals<br/>pd.get_dummies]
    N --> O[Fill NaN with 0]

    O --> P[Train/test split<br/>train_test_split]
    P --> Q[Train XGBoostClassifier]

    Q --> R[Predict proba & label<br/>y_pred_proba, y_pred_label]
    R --> S[Evaluate AUC, classification_report,<br/>confusion matrix]

    S --> T[End]
```
