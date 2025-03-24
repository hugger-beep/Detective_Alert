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
    "related_types": ["LargeFileUpload", "UnusualDownload", "UnusualVPCFlow"],
    "min_findings": 2,
    "score_threshold": 6
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
