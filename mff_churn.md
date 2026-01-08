flowchart TB
    A["Start"] --> B["Load df_nps and inspect last rows"]
    B --> H["Apply season_rule to future Season_impact"]
    H --> J["Define feature matrices for historical and future data"]
    J --> K{"Choose model"}
    K -- Random Forest --> L["Set(n_estimators=300,
        max_depth=3,
        random_state=42)"]
    K -- Support Vector --> M@{ label: "Set (kernel='rbf', C=1.0, epsilon=0.1)" }
    M --> N["Predict future values using future data"]
    L --> N
    N --> Q["Compute Overall result constant"]
    Q --> V["End"]

    M@{ shape: rect}
