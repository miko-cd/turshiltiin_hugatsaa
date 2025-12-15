mermaid
```
flowchart TD
    A[Start] --> B[Connect SQL Server]
    B --> C[Read numeric data<br/>APRU_increased → df_num]
    B --> D[Read categorical PREV<br/>AR_gsm_mobi_active → df_categorical_prev]
    B --> E[Read categorical CUR<br/>AR_gsm_mobi_active→ df_categorical_cur]

    E --> F[Compute tenure<br/>today - c_initial_date → tenure_cur]
    D --> G[Merge prev/cur categorical<br/>on c_isdn → df_cat]
    F --> G

    G --> H[Build df_cat_clean<br/>age, gender, tenure,<br/>cur/prev package/payment/marketing/handset]
    H --> I[Create change flags<br/>change_handset, change_payment]

    C --> J[Join numeric + categorical→ df_all]
    I --> J

    J --> K[Clean & select columns<br/>drop cur/prev avg_*, fix age=-1, remove inf/NaN]
    K --> L[Define features<br/>num_cols + cat_cols + delta_ARPU label]
    L --> M[One-hot encode categoricals<br/>pd.get_dummies]
    M --> N[Build X, y]

    N --> O[Train/test split<br/>train_test_split]
    O --> P[Train XGBoost Regressor]
    P --> Q[Evaluate R2, MAE]
    P --> R[SHAP analysis & feature importance]

    Q --> S[End]
    R --> S
```
