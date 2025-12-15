```mermaid
flowchart TD
    A[Start] --> C[Connect to SQL Server<br/>Load APRU_increased]
    C --> D[Merge prev & current snapshots]
    D --> F[Create change flags<br/>change_handset, change_payment]
    F --> G[Build full dataset]
    G --> H[Define numeric & categorical features]
    H --> I[Encode categoricals and scale numericals]
    I --> J[Train/test split]
    J --> K[Train XGBoost regressor]
    K --> L[Evaluate model<br/>R², MAE]
    L --> M[Compute SHAP values<br/>feature importance share]
    M --> O[End]
  ```
