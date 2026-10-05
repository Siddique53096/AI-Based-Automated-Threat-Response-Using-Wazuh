# AI-Based Automated Threat Response Using Wazuh

Final Year Project — BS Cyber Security, Riphah International University

## Overview

This project extends Wazuh with cyber threat intelligence and AI-assisted alert investigation. It aims to help SOC analysts prioritise security alerts, understand the evidence, and respond through a controlled approval workflow.

A local machine-learning model scores incoming alerts. Alerts that need further review reach an LLM agent for investigation and response recommendations. The integration service checks each recommendation against the response policy before allowing execution.

**Status:** Under development. The features below describe the planned prototype. Evaluation results will be added after testing.

## Main Features

- Monitor Windows and Kali Linux endpoints through Wazuh agents.
- Collect Sysmon, authentication, process and network activity.
- Support file integrity monitoring, security configuration checks and vulnerability monitoring.
- Match IP addresses, domains, URLs and file hashes against threat intelligence.
- Integrate AlienVault OTX, MISP and an OpenCTI prototype.
- Score alerts using a local ML pre-filter and explain scores with SHAP.
- Use an LLM agent for triage, enrichment and investigation of related alerts.
- Map activity to MITRE ATT&CK where the evidence supports a mapping.
- Apply response policies with analyst approval for medium, high and critical-risk actions.
- Maintain incident records, evidence, timelines and audit logs.
- Generate incident reports and retain reviewed analyst feedback.

## System Workflow

1. Endpoint agents send activity to the Wazuh Manager.
2. Wazuh decodes events, applies detection rules and correlates related alerts.
3. The custom Integrator sends eligible source alerts to the FastAPI service.
4. The service extracts indicators, performs CTI lookups and builds model features.
5. The ML pre-filter scores alerts:
   - Low scores: archive with evidence for later review.
   - Medium scores: send for Level-1 triage.
   - High scores: prioritise investigation and open an incident.
6. The LLM agent investigates relevant alerts and returns a structured verdict.
7. The service validates the recommendation and applies the action-risk policy.
8. Permitted endpoint actions run through Wazuh Active Response.
9. Verdicts, approvals and outcomes are recorded in Wazuh and incident records.

ML alert-score bands and response-action risk tiers are separate decisions.

## Response Policy

| Action risk | Planned behaviour |
|---|---|
| Low | Allow configured automatic actions such as notifications, watchlist updates and incident creation. |
| Medium | Require analyst approval for actions such as file quarantine or additional evidence collection. |
| High / Critical | Require explicit approval with a recorded reason before blocking an IP, stopping a process or disabling an account. |

Rejected or expired requests do not execute. Protected hosts cannot be blocked or isolated. The LLM does not hold Wazuh execution credentials.

## Technology Stack

| Area | Planned tools |
|---|---|
| SIEM and endpoint monitoring | Wazuh, Sysmon |
| Integration service | Python, FastAPI |
| Threat intelligence | AlienVault OTX, MISP, OpenCTI |
| Machine learning | scikit-learn, XGBoost or LightGBM |
| Model explanations | SHAP |
| AI investigation | LLM API with controlled tool access |
| Threat mapping | MITRE ATT&CK |
| Deployment | Docker and Linux |

The final ML classifier will be selected through validation.

## Datasets and Evaluation

The ML pre-filter will use AIT-ADS and labelled lab alerts, with selected OTRF and EVTX samples for additional coverage.

GUIDE incident grades will support a separate triage-evaluation model. These grades will remain separate from Wazuh event labels.

Testing will include model evaluation, synthetic IOC matching, end-to-end lab scenarios, approval checks, outage tests and comparison against default Wazuh. Measurements will cover accuracy, false positives, detection and response time, triage quality, latency and API cost.

## Project Team

| Team member | Main responsibility |
|---|---|
| Zain Ali Syed | Endpoint monitoring, Wazuh deployment, detection rules and Wazuh–AI integration |
| Siddique Akbar | CTI integration, IOC lifecycle management and threat enrichment |
| Hashim Ali Khan | ML pre-filter, LLM SOC agent, incident management and response workflow |

Integration, testing and documentation are shared team responsibilities.

**Supervisor:** Arslan Ali Khan  
**Institution:** Riphah International University  
**Program:** BS Cyber Security

## Testing Scope

Testing uses controlled scenarios and synthetic indicators inside an isolated, authorised lab environment.
