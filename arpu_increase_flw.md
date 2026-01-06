mermaid 
````
    flowchart TD

    A[Start] --> B[Connect SQL Server]
    B --> C[Read APRU_increased]
    B --> D[Read categorical PREV]
    B --> E[Read categorical CUR]

    E --> F[Compute tenure]
    D --> I[Merge prev/cur categorical<br/>on c_isdn → df_cat]
    F --> I

    C --> J[Join df_num + df_cat<br/>on c_isdn → df_all]
    I --> J

    J --> K[delta_ARPU > 5000 → 1/0]
    K --> L[Preprocessing & Encode categoricals]


    L --> P[Train/test split<br/>Train XGBoostClassifier]
    P --> R[Predict new month]

    R --> T[End]
````
