# econ5200-lab03-eda-diagnostics
## Pipeline Health Check — EDA, Corruption & Distribution Shift

### Objective
Diagnose and remediate data-quality failures in a country-level panel dataset and quantify training-to-inference distribution shift, producing reusable validation utilities and a monitoring dashboard for ongoing pipeline health.

### Methodology
- **Exploratory corruption audit:** Used exploratory data analysis alone (summary statistics, range checks, distribution plots, and cross-variable consistency checks) to detect five planted data-quality issues without prior knowledge of their location or type.
- **Targeted remediation:** Corrected each issue with a rule matched to its cause: removed or flagged impossible negative GDP values, converted life expectancy from months to years, deduplicated repeated country-year records, standardized percentage fields stored in mixed units (0–1 vs. 0–100), and reconciled a GDP unit mismatch across records.
- **Distribution shift measurement:** Computed the Population Stability Index (PSI) between training and inference samples to quantify drift in key features.
- **Tool benchmarking:** Compared the manual EDA workflow against an automated ydata-profiling report and documented the classes of issues each approach surfaced and missed.
- **Reusable tooling:** Packaged the validation logic into `eda_utils.py`, a module with functions for impossible-value detection, PSI calculation, and dataset summaries.
- **Monitoring:** Built an interactive pipeline-health dashboard to surface data-quality flags and drift metrics in one view.

### Key Findings
- All five planted corruptions were identified and resolved through EDA, which shows that disciplined exploratory checks can catch unit, range, and duplication errors before modeling.
- GDP had a **PSI of 1.8609** between training and inference data. This is far above the conventional 0.25 threshold for significant shift. Models trained on this feature would likely degrade in production without retraining or recalibration.
- Automated profiling and manual EDA were complementary rather than interchangeable:
  - ydata-profiling efficiently surfaced [e.g., missingness, duplicates, correlations].
  - Manual, domain-informed checks were required to catch [e.g., semantically wrong but statistically plausible values such as unit mismatches].
- The resulting utilities and dashboard give the workflow a repeatable foundation for validating future data loads.
