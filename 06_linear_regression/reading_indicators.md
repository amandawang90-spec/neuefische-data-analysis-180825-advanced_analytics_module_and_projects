#### R2 Domain Values

| Domain                               | Typical R²  | Why                       |
| ------------------------------------ | ----------- | ------------------------- |
| Physics, engineering                 | 0.9+        | Systems are deterministic |
| Economics, marketing, social science | 0.1–0.4     | Human behavior is noisy   |
| Finance                              | < 0.1 often | Markets are unpredictable |


#### How to Evaluate R2 Reliability

| Metric                | What It Tells You                | Typical Method                 |
| --------------------- | -------------------------------- | ------------------------------ |
| **Train vs. Test R²** | Overfitting or underfitting      | Split or cross-validation      |
| **RMSE / MAE**        | Prediction error in real units   | Compare on validation set      |
| **Residual plots**    | Model assumptions hold           | Check randomness               |
| **Adjusted R²**       | Penalizes adding weak predictors | Already reported               |


#### Correlation by Domain

| Field / Domain                     | Typical Correlation (r) | Interpretation                                    |
| ---------------------------------- | ----------------------- | ------------------------------------------------- |
| Physics / Chemistry                | 0.95+                   | Extremely strong (systems are physical laws)      |
| Biology / Medicine                 | 0.6–0.8                 | Strong — living systems have biological variation |
| Psychology / Marketing / Economics | 0.2–0.5                 | Moderate — human behavior is noisy                |
| Finance                            | 0.1–0.3                 | Weak but often still useful                       |
| Social Science / Education         | 0.1–0.4                 | Moderate — context-sensitive                      |

An r = 0.3 might be “weak” in physics,
but in behavioral economics, that could mean a major effect with real business impact.

## Summarizing Stats to Business

| Concept              | Key Takeaway                                         |
| -------------------- | ---------------------------------------------------- |
| Correlation strength | Context-dependent — physics ≠ marketing              |
| R² vs. r             | R² shows variance explained; r shows linear strength |
| Significance         | Statistical ≠ business significance                  |
| Interpretation       | Always tie back to decision impact and cost-benefit  |
