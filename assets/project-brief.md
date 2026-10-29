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

These are inspected-split results, not an independent external validation. The audit identified 21,080 evaluation rows with identical feature vectors in training; the original artifact training membership remains unproven.

## Fresh model findings

New same-corpus models used a predeclared protocol and recorded partitions with zero identical-vector overlap. The fresh forest achieved internal test F1 0.999863, PortScan-to-DDoS F1 0.730643, and DDoS-to-PortScan F1 0.001234. In the last condition it detected 56 of 90,694 attacks; the baseline reached F1 0.838760. These separate experiments do not constitute independent external validation or isolate capture effects from attack-family effects. The dashboard model is unchanged.

## Reliability and deployment
The final recorded Windows suite passed 72 tests. The updated isolated CI selection passed 65 tests locally in 16.31 seconds with 69% combined statement/branch coverage over the current source; that scope differs from the full local suite and earlier Unit 7 coverage. Tests found malformed model artifacts, unsupported labels and duplicate normalized headers; controlled handling and rejection were added. Hosted CI is checked on the final integration pull request.

The Docker image packages the application and dependencies. Dataset/model assets are mounted read-only. Configuration uses ANOMALY_DATA_DIR and ANOMALY_MODEL_PATH. The application listens on container port 8501; the verified local setup exposed it through host loopback. HTTP health indicates liveness, not readiness of data/model assets.

The restricted non-root Docker run completed the real-data comparison. The 108 bounded backend operations preserved reference confusion counts; they do not measure browser-user capacity. A local restore of 91 files passed hash checks and reproduced the reference application results in about 6.48 seconds from restore start on the same machine and existing Python environment. Pillow advisories remain unresolved due to a dependency compatibility constraint. Offsite and fresh-machine recovery remain untested.

## Final presentation
https://drive.google.com/file/d/1u0YgdYLryu9okjnbDBIemov_FUEC6TkI/view?usp=sharing

Slides: MSIT 5910-01 Unit 8 Final Presentation Slides.pdf, available through the portfolio.

## Scope and future work
No live packet capture, automatic blocking, authentication, TLS termination, durable audit database, independent user acceptance study, or production deployment is claimed. Future work prioritizes genuinely independent data, transfer-failure diagnosis, dependency remediation, usability studies, and operational security controls.

## Source access
https://github.com/chrishazell13/ml-network-anomaly-detection
The repository is private and requires authorized collaborator access.
