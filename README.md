# Detective_Alert
# AWS Detective Alert and Correlation Sample

## Overview
This system provides advanced security finding correlation and alerting for AWS Detective. It processes various types of Detective events, correlates related findings, and generates intelligent alerts based on severity and correlation patterns.

## Features
- Real-time processing of Detective findings
- Advanced correlation of related security events
- Configurable correlation rules and thresholds
- Multiple event type handling (findings, investigations, alerts)
- Automated alert generation via SNS
- Comprehensive logging and error handling

### Components
1. **FindingCorrelator**: Handles security finding correlation
2. **DetectiveAnalyzer**: Processes Detective events and generates alerts
3. **Lambda Handler**: Main entry point for AWS Lambda execution

### Event Types Handled
- Detective Finding
- Detective Finding Group
- Finding Group State Change
- Investigation State Change
- Detective Alert
- Member Invitation

## Correlation Logic

### Correlation Rules
The system includes four default correlation types:

1. **PRIVILEGE_ESCALATION**
   ```json
   {
       "related_types": ["UnauthorizedAccess", "IAMPolicyChange", "RootCredentialUsage"],
       "min_findings": 2,
       "score_threshold": 7
   }

2. **DATA_EXFILTRATION**
   ```json
   {
       "related_types": ["UnauthorizedAccess", "IAMPolicyChange", "RootCredentialUsage"],
       "min_findings": 2,
       "score_threshold": 7
   }

3. **CREDENTIAL_COMPROMISE**
   ```json
   {
    "related_types": ["PasswordCracking", "BruteForceAttempts", "UnauthorizedAPICall"],
    "min_findings": 3,
    "score_threshold": 8
   }

4. **PERSISTENCE_ATTEMPT**
   ```json
   {
    "related_types": ["IAMUserCreation", "UnauthorizedKeyCreation", "SecurityToolDisabled"],
    "min_findings": 2,
    "score_threshold": 7
   }

### Severity Scores
  
   {
    'CRITICAL': 5,
    'HIGH': 4,
    'MEDIUM': 3,
    'LOW': 1
   }

### Correlation Window
1. Default: 4 hours
2. Configurable via update_correlation_window(hours)

### Alert Decision Logic
1. Correlated Findings Alert
     Triggered when:
        Minimum number of related findings is met,
        Total score exceeds rule threshold,
        Findings occur within correlation window

2. Individual Finding Alert
     Triggered when:
        Single finding score ≥ 4 (HIGH or CRITICAL severity),
        No correlation pattern detected

3. Finding Group Alert
     Triggered when:
        Total group score ≥ 7 OR,
        ≥ 2 high severity findings in group


4. Investigation State Change Alert
     Triggered for states:
        IN_PROGRESS,
        ESCALATED
   
## Configuration
### Environment Variables
DETECTIVE_GRAPH_ARN: ARN of Detective graph
SNS_TOPIC_ARN: ARN of SNS topic for alerts
