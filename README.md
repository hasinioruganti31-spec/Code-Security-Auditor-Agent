CODE SECURITY AUDITOR AGENT

An AI-powered security auditing workflow built with n8n that analyzes code changes, detects security regressions, evaluates their potential impact, checks project-specific security policies, and generates prioritized remediation recommendations.

Overview

CODE SECURITY AUDITOR AGENT is designed to answer a key security question:

“Did this code change introduce a security risk, weaken an existing security control, or violate the project's security policies?”

Instead of analyzing only the current version of code, the agent compares:

Old Code
New Code
Change Description
Security Policy
Project Context

The AI then performs security analysis and produces a structured security report.

Problem

Traditional code security analysis often focuses on finding vulnerabilities in the current codebase.

However, a security issue can be introduced when a developer changes previously secure code.

For example:

// OLD CODE
const q = "SELECT * FROM users WHERE id = ?";
db.query(q, [req.query.id]);

A change might replace it with:

// NEW CODE
const q = "SELECT * FROM users WHERE id = " + req.query.id;
db.query(q);

The new implementation introduces a potential SQL injection vulnerability and weakens the previous security control.

The CODE SECURITY AUDITOR AGENT focuses specifically on identifying these types of security regressions caused by changes.

Key Features
🔍 Code Change Analysis

Compares old and new code to understand what changed.

🛡️ Security Regression Detection

Identifies security weaknesses introduced by the new implementation.

🚨 Vulnerability Detection

Detects potential issues such as:

SQL Injection
Cross-Site Scripting
Command Injection
Authentication weaknesses
Authorization issues
Insecure input handling
Sensitive data exposure
Security control removal or weakening
📋 Security Policy Analysis

Checks the code change against project-specific security policies.

Example:

1. Database queries must use parameterized queries.
2. User input must be validated.
3. Security controls must not be weakened.
💥 Change-Impact Analysis

Determines potentially affected:

Users
Data
Components
APIs
Security controls
📊 Risk Calculation

Calculates a security risk score based on:

Finding severity
Number of findings
Security regression detection
🎯 Risk Prioritization

Orders security findings so that the most severe issues can be addressed first.

🔧 Remediation Recommendations

Provides recommended fixes and secure code examples where appropriate.

📝 Structured Security Report

Produces machine-readable security analysis that can be used by downstream automation.

🧠 Audit History Correlation

The architecture supports future integration with previous audit results to identify recurring security problems and related historical findings.

Workflow Architecture
                 ┌──────────────────────┐
                 │      User Input       │
                 │                      │
                 │ Old Code             │
                 │ New Code             │
                 │ Change Description    │
                 │ Security Policy      │
                 │ Project Context      │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │     n8n Workflow     │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │       AI Agent       │
                 │  Security Analysis  │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Structured Output    │
                 │      Parser          │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │   Risk Calculator    │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Risk Prioritization  │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │ Security Report      │
                 └──────────────────────┘
Analysis Pipeline

The agent performs the following stages:

1. Change Analysis

Determines what changed between the old and new implementations.

2. Security Analysis

Examines the new implementation for potential security weaknesses.

3. Regression Detection

Determines whether a previously existing security control has been removed or weakened.

4. Policy Analysis

Checks the change against the supplied security policies.

5. Impact Analysis

Identifies potentially affected components, users, and data.

6. Risk Calculation

Calculates a normalized security risk score.

7. Risk Prioritization

Prioritizes findings according to severity.

8. Remediation

Provides recommended corrective actions.

9. Security Decision

Produces a final decision such as:

SECURITY_REGRESSION

or

NO_SECURITY_REGRESSION
Example Input
OLD CODE:

const q = "SELECT * FROM users WHERE id = ?";
db.query(q, [req.query.id]);


NEW CODE:

const q = "SELECT * FROM users WHERE id = " + req.query.id;
db.query(q);


CHANGE DESCRIPTION:

Changed the database query implementation.

OR
Audit this change.

OLD CODE:
const query = 'SELECT * FROM users WHERE id = ?';
db.query(query, [id]);

NEW CODE:
const query = 'SELECT * FROM users WHERE id = ' + req.query.id;
db.query(query);
console.log(result);

CHANGE DESCRIPTION:
The database query was changed from a parameterized query to string concatenation using a request parameter.

Please compare this change with the previous audit history available in the workflow.
Identify whether this is a security regression and whether it relates to any previous audit findings.
Do not assume affected users, data, or components unless supported by the provided information.

SECURITY POLICY:

1. Database queries must use parameterized queries.
2. User input must be validated before being used.
3. Security controls must not be weakened.


PROJECT CONTEXT:

Node.js web application with a user lookup endpoint.
Example Analysis

The agent can identify:

Security Regression: TRUE

Risk: Critical

Security Issue:
SQL Injection

Reason:
The new implementation directly concatenates user-controlled
input into a SQL query instead of using a parameterized query.

Policy Violations:
- Parameterized queries: Violated
- User input validation: Violated
- Security controls must not be weakened: Violated

It can also identify potentially affected data and components and provide a remediation such as restoring parameterized queries.

Example Output Structure
{
  "security_regression": {
    "detected": true,
    "reason": "A parameterized SQL query was replaced with string concatenation."
  },
  "overall_risk": "Critical",
  "security_score": 10,
  "findings": [
    {
      "title": "SQL Injection via String Concatenation",
      "severity": "Critical",
      "category": "Injection",
      "recommended_remediation": "Use parameterized queries."
    }
  ],
  "security_decision": "SECURITY_REGRESSION",
  "priority_actions": [
    "Restore parameterized query implementation",
    "Validate user-controlled input",
    "Review similar database queries"
  ]
}
Technology Stack
n8n — Workflow automation
Google Gemini — AI security analysis
n8n AI Agent — Security reasoning and analysis
Structured Output Parser — Consistent security report format
JavaScript — Risk calculation and prioritization
GitHub — Version control and project documentation
Project Structure
CODE-SECURITY-AUDITOR-AGENT/
│
├── README.md
│
├── n8n/
│   └── code-security-auditor-agent.json
│
├── prompts/
│   └── security-auditor-system-prompt.md
│
├── examples/
│   ├── vulnerable-sql-injection.md
│   └── sample-security-report.json
│
├── docs/
│   └── architecture.png
│
└── .gitignore
Security Approach

The agent is designed for defensive security analysis.

It does not need to execute or exploit the submitted code. Instead, it analyzes the provided code and change information to identify potential security problems.

The analysis also distinguishes between:

Confirmed findings based on provided evidence
Assumptions
Potential impacts
Recommended remediation

The agent should not invent project architecture, security policies, vulnerabilities, or historical findings that were not provided.

Future Enhancements

Potential future improvements include:

🔄 GitHub pull-request integration
📁 Automatic repository/code-change ingestion
🧠 Persistent security audit history
🔎 Similarity detection for recurring vulnerabilities
📈 Security trend dashboards
🔔 Slack/email security alerts
📝 Automatic security review comments
🔐 Organization-specific security policies
📊 Security metrics and reporting
🔗 Integration with existing SAST/DAST tools
Use Cases

The project can be used for:

Secure code review
Pull-request security analysis
Security regression detection
Developer security feedback
Policy compliance checking
Change-impact assessment
Automated security auditing
Disclaimer

This project is intended for defensive security analysis and educational purposes.

The generated findings are automated analysis and should be reviewed by qualified security or engineering personnel before being treated as a definitive security assessment.

Author

CODE SECURITY AUDITOR AGENT

An n8n-based AI workflow for analyzing security risks introduced by software changes.
