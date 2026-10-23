# Machine Learning Assisted Network Anomaly Detection
Christopher Hazell | MSIT Capstone

## Purpose and implementation
A local, batch-analysis prototype for comparing a transparent threshold baseline with a Random Forest classifier on stored network flow records. Python, pandas, scikit-learn, and Streamlit form the integrated application. Shared modules handle ingestion, preprocessing, prediction, evaluation, and JSON/CSV exports.

## Core logic
The rule baseline examines Flow Packets/s, Fwd Packets/s, SYN Flag Count, RST Flag Count, and Total Fwd Packets. Thresholds are based on the 90th percentile of benign training records. At least two strict threshold exceedances flag a record as an attack. The existing Random Forest artifact uses 100 trees. Labels are excluded from the 78 input features.

## Evaluation evidence
The inspected evaluation split contains 102,362 records from CICIDS2017 Friday PortScan and DDoS data: 44,996 benign records and 57,366 attack records.

| Measure | Rule baseline | Random Forest |
| --- | ---: | ---: |
| True negatives | 40,649 | 44,995 |
| False positives | 4,347 | 1 |
| False negatives | 36,185 | 5 |
| True positives | 21,181 | 57,361 |
| F1 | 0.511038 | 0.999948 |

These are inspected-split results, not an independent external validation. Related capture records and unreconstructed training provenance limit generalization claims.

## Reliability and deployment
The final Windows suite passed 62 tests with 76% combined statement/branch coverage. A Linux container run passed 55 overlapping dataset-independent tests. Testing exposed malformed and non-predictor model artifacts, leading to improved controlled error handling. Unit 5 recorded a successful GitHub Actions run; a hosted Unit 7 run is not claimed.

The Docker image packages the application and dependencies. Dataset/model assets are mounted read-only. Configuration uses ANOMALY_DATA_DIR and ANOMALY_MODEL_PATH. The application listens on container port 8501; the verified local setup exposed it through host loopback. HTTP health indicates liveness, not readiness of data/model assets.

## Scope and future work
No live packet capture, automatic blocking, authentication, TLS termination, durable audit database, independent user acceptance study, or production deployment is claimed. Future work includes reconstructing training provenance, external and time-separated evaluation, usability studies, and operational security controls.

## Source access
https://github.com/chrishazell13/ml-network-anomaly-detection
The repository is private and requires authorized collaborator access.
