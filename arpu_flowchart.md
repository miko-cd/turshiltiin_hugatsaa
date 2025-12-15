```mermaid
flowchart TD
    A[Start] --> B[Connect to SQL Server<br/>Load APRU_increased]
    B --> C[Prepare user attributes<br/>Compute tenure from initial date]
    C --> D[Merge prev & current snapshots<br/>Create prev_* and cur_* columns]
    D --> E[Create change flags<br/>change_handset, change_payment]
    E --> F[Join with APRU_increased<br/>to build full dataset]
    F --> G[Clean data<br/>fix ages, drop bad rows]
    G --> H[Define numeric & categorical features]
    H --> I[Encode categoricals<br/>Build X and y=delta_ARPU]
    I --> J[Train/test split]
    J --> K[Train XGBoost regressor]
    K --> L[Evaluate model<br/>R², MAE]
    L --> M[Compute SHAP values<br/>Global feature importance]
    M --> N[Export analysis]
    N --> O[End]
  ```
