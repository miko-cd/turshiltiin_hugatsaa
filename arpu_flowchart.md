mermaid
flowchart TD
    A[Start] --> B[1. Niit hereglegchdiin<br/>ARPU_increased table uusgeh<br/>- All features (cat, num)<br/>- avg_ARPU herhen oorchlogdson]
    B --> C[2. Stable 6 month users: 1,303,210<br/><br/>Segment by delta_ARPU:<br/>- delta_ARPU &gt; 0 → 670,143 users<br/>- delta_ARPU ≤ 0 → 633,067 users]
    C --> D[3. All preprocessing steps<br/>- Cleaning<br/>- Encoding<br/>- etc.]
    D --> E[4. Model: XGBRegressor<br/>- n_estimators = 1000<br/>- learning_rate = 0.03<br/>- max_depth = 7<br/>- subsample = 0.8<br/>- colsample_bytree = 0.8<br/>- min_child_weight = 3<br/>- reg_alpha = 0.1<br/>- reg_lambda = 1.0<br/>- objective = 'reg:squarederror'<br/>- random_state = 42<br/>- tree_method = 'hist']
    E --> F[5. Performance evaluation<br/>- R-Squared (R2) metric in Linear Regression<br/>- Graphic: R2 = 0.7793, MAE = 1735.5268<br/>- Tailbar nemeh (visual explanation)]
    F --> G[End]
    
