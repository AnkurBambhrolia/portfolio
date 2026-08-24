# Surgical Ramp-Up Capacity Stress Test

## Executive Summary

This project evaluates how a hospital can restore elective surgical volume while protecting PACU and inpatient capacity.

Using historical patient-flow patterns and Monte Carlo simulation, three ramp-up strategies were tested across normal operations and stressed conditions. The final recommendation is a **phased ramp of 12 additional elective cases over a 13-week quarter**, released as **+2 cases in weeks 1–4, +3 cases in weeks 5–8, and +7 cases in weeks 9–13**.

The main finding is that **inpatient capacity—not PACU capacity—is the primary operational constraint**. Longer inpatient length of stay and increased ED admissions create substantially more system pressure than the additional elective volume itself.

## Business Question

> How can elective surgical volume be restored over the next quarter while remaining resilient to longer inpatient stays and an ED surge?

## Analytical Approach

The analysis combines historical surgical and inpatient encounter data with Monte Carlo simulation.

Three scheduling strategies were evaluated:

1. **Immediate ramp** — restore volume using the historical scheduling pattern.
2. **Phased ramp** — introduce volume gradually across the quarter.
3. **Capacity-aware ramp** — preferentially schedule added cases into historically lower-pressure operating windows.

The simulation:

- reconstructs PACU occupancy in 15-minute intervals;
- reconstructs inpatient census in hourly intervals;
- resamples historical patient-flow templates and baseline 13-week quarters;
- evaluates added elective volume from 0 to 20 cases;
- uses 1,000 Monte Carlo iterations per evaluated scenario;
- stress-tests inpatient LOS +15%, ED admissions +20%, and combined stress.

## Key Findings

### Inpatient capacity is the primary constraint

PACU pressure remains relatively stable across the tested ramp range, while inpatient exposure increases as added surgical volume accumulates over multi-day hospital stays.

Historical inpatient census reference levels used for planning:

- **33 patients:** P90 high-census reference
- **42 patients:** historical maximum observed in the dataset

These are planning reference points, not confirmed physical bed-capacity limits.

### Phased +12 remains within the selected normal-condition guardrails

A phased ramp of 12 cases represents an approximately **10.6% increase** over the estimated quarterly baseline of 113 elective cases.

Under normal conditions:

- Probability inpatient census exceeds 42: **13.7%**
- P90 peak inpatient census: **43**
- P90 additional time at census 33+: **73 hours**
- P90 PACU time above four occupied bays: **45 minutes**

### System stress matters more than scheduling design

For phased +12:

| Operating condition | Probability census exceeds 42 | P90 peak census |
|---|---:|---:|
| Normal | 13.7% | 43 |
| LOS +15% | 47.0% | 49 |
| ED admissions +20% | 32.0% | 46 |
| Combined stress | 67.8% | 52 |

The combined scenario shows that severe inpatient stress cannot be solved by scheduling strategy alone.

## Recommendation

Proceed with a **monitored phased ramp of 12 additional elective cases over 13 weeks**:

- **Weeks 1–4:** +2 cases
- **Weeks 5–8:** +3 cases
- **Weeks 9–13:** +7 cases

Progression should be conditional on inpatient census, LOS, ED demand, and PACU staffing.

A simple operating framework is:

- **Green:** continue the ramp
- **Yellow:** hold and review
- **Red:** pause and escalate

## Validation

A faster NumPy-based stress-test engine was validated against the original capacity-frontier simulation. The implementations remained within predefined tolerances and preserved the same operational conclusions.

## Tools

- Python
- pandas
- NumPy
- Matplotlib
- Google BigQuery
- Monte Carlo simulation

## Repository Structure

```text
surgical-ramp-up/
├── README.md
├── notebooks/
│   └── surgical_ramp_up_simulation.ipynb
└── presentation/
    └── Surgical_Ramp_Up_Recommendation.pdf
```

## Data Availability

The underlying healthcare dataset is not included in this repository. The notebook references the course/demo BigQuery dataset used for the analysis and retains saved outputs so the analytical workflow and results can still be reviewed without direct data access.

The notebook uses a placeholder for the Google Cloud billing/execution project. To rerun the analysis, replace:

```python
RAMP_PROJECT_ID = "YOUR_GCP_PROJECT_ID"
```

with an authorized Google Cloud project ID that can access the required BigQuery dataset.

## Notes

This project is designed as an operational decision-support simulation, not as a patient-level predictive model. The census thresholds used in the analysis are historical planning references rather than confirmed staffed or licensed bed-capacity limits.
