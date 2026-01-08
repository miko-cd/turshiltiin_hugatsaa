flowchart TB
    A["Start"] --> B["Prepare training data (Pop_active + Pop_inactive) from SQL"]
    B --> D["Data cleaning & feature engineering & encode/scale"]
    D --> G@{ label: "Baseline XGBClassifier \\objective='binary:logistic',random_state=42\\" }
    G --> H["Train baseline model & Evaluate tuned model"]
    H --> J{"Hyperparameter 
    tuning step"}
    J -- XGBoost tuned --> K["XGBClassifier tuned \learning_rate=0.3,\n_estimators=1000,
    \max_depth=5, etc\"]
    K --> L["Train tuned model & Evaluate tuned model"]
    J -- Compare results --> N["Compare baseline vs tuned"]
    N --> O["Select final model 
    & Save model"]
    O --> S["Predict Nov-pop churn probability"]
    S --> V["End"]

    G@{ shape: rect}
