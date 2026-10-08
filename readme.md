Phishing Email Analyzer & Security Awareness Trainer 🛡️📧
Python Version License: MIT Test Coverage Security Focus

An interactive, defensive cybersecurity application built in Python 3.11+ that performs static, offline risk assessments of raw .eml email files. It combines an automated explainable scoring engine with a sleek Sepia & Espresso Web GUI specifically designed for phishing awareness training, SOC triage analysis, and security education.

Phishing Email Inspector Banner

🎯 Phishing Awareness & Educational Use Case
Phishing remains the #1 initial attack vector in cyber breaches. While automated email security gateways block standard spam, human awareness is the critical last line of defense.

This tool transforms raw .eml header and body data into plain-language security findings backed by concrete email proof, allowing students, IT staff, and employees to:

Learn How Attackers Deceive Users: Visualize display-name spoofing, From vs. Reply-To mismatches, and href link tricks.
Inspect Evidence Safely: Interrogate real-world phishing headers, URLs, and attachment hashes without opening live links or executing malicious code.
Understand Risk Scoring: See how individual indicators combine into a weighted Risk Index (0–100) with clear verdicts (Likely Legitimate, Suspicious, Likely Phishing).
📸 Screenshots & UI Showcase
1. Sepia & Earthy Brown Security Dashboard
Web GUI Main Dashboard Overview The modern Web GUI features categorized sample lists (Real-World Phishing vs. Synthetic Test Suite) sorted top-to-bottom by Risk Index.

2. Evidence-Backed Risk Findings
Interactive Email Evidence Proof View Every flagged indicator includes exact email snippet proof (e.g. extracted URL mismatches, urgency keywords, or embedded password forms).

3. Rich Terminal Interface (CLI)
Rich Terminal Summary Command-line interface with color-coded risk summaries, tabular findings, and defanged IOC export tables.

🔍 What Makes an Email Suspicious? (Educational Guide)
Below is an overview of the core security checks performed by the analyzer, serving as a reference guide for phishing awareness:

🚩 1. Header & Envelope Mismatches
SPF / DKIM / DMARC Failures: Email fails cryptographic domain authentication, indicating the sender domain was forged.
From vs. Reply-To Mismatch: The display header says support@paypal.com, but replies are routed to hacker-inbox@attacker-domain.top.
Display Name Spoofing: Display name contains a trusted brand or email address (e.g. IT Helpdesk <evil@external.org>).
Return-Path Discrepancies: Bounce-back path points to an unrelated external domain.
🚩 2. URL & Link Evasion Tactics
Visual vs. href Mismatch: Link text displays https://bank.com/login, but the underlying href destination points to hxxp://phish-bank[.]top.
Brand Typosquatting (Levenshtein Distance): Detects lookalike domains (e.g. paypa1-security.top mimicking paypal).
Punycode / IDN Homograph Attacks: Internationalized domains using Cyrillic characters to impersonate Latin letters (xn--...).
URL Shorteners & Raw IPs: Use of bit.ly, tinyurl.com, or raw IP addresses (http://192.168.1.1/login) to conceal destinations.
Suspicious High-Risk TLDs: Links resolving to .top, .xyz, .zip, .cc, or .tk.
3. Social Engineering & Body Indicators
Urgency & Threat Pressure: Phrases demanding immediate action within 24 hours under threat of account closure or legal action.
Credential & Payment Requests: Solicitations for passwords, PINs, SSNs, or credit card updates.
Embedded Password Forms: HTML emails containing <form action="..."> and <input type="password"> elements.
Hidden CSS Text: White text on white background (color: #fff; font-size: 0px) used to bypass naive spam filters.
🚩 4. Dangerous Attachment Patterns
High-Risk Executable Formats: Files ending in .exe, .scr, .bat, .ps1, .vbs, .js, .iso, .docm, or .xlsm.
Double-Extension Evasion: Trick filenames like invoice.pdf.exe or report.docx.vbs.
MIME Header Mismatches: Claiming to be an image/png while having an executable binary payload.
Cryptographic Hashes: Static extraction of SHA-256 hashes for safe SOC blocklist checking.
🧪 Included Sample Email Corpora
The application ships pre-loaded with 17 sample .eml files categorized into two side-by-side suites:

Suite	Count	Score Range	Description
Real-World Phishing Corpus	6	65.0 - 100.0	Real phishing samples sourced from open repositories (Bradesco Livelo points phish, Microsoft sign-in alert, solar imposter phish). Zero attachments included.
Synthetic Test Suite	11	0.0 - 100.0	Controlled test cases covering clean internal emails, marketing newsletters, softfails, typosquatting, credential harvesting, and dangerous attachments.
🏗️ Architecture & Data Flow
flowchart TD
    EML[Raw .eml File] --> Parser[EML Parser - policy.default]
    Parser --> EmailData[EmailData Object]
    Config[YAML / JSON Rules Config] --> ScoringEngine
    Config --> Checks

    EmailData --> HC[Header Check Module]
    EmailData --> UC[URL Check Module]
    EmailData --> BC[Body Check Module]
    EmailData --> AC[Attachment Check Module]

    HC --> Findings[List of Findings + Evidence Proof]
    UC --> Findings
    BC --> Findings
    AC --> Findings

    Findings --> ScoringEngine[Scoring Engine]
    EmailData --> ScoringEngine
    ScoringEngine --> Result[AnalysisResult + Risk Index]

    Result --> Output{Output Selection}
    Output -->|Web GUI| Flask[Flask Web Application - Sepia Theme]
    Output -->|CLI Terminal| Rich[Rich Terminal Reporter]
    Output -->|Export JSON| JSON[Defanged Machine JSON]
    Output -->|Export HTML| HTML[XSS-Sanitized HTML File]
⚙️ Installation
# 1. Clone repository
git clone https://github.com/oponder2000/phishing-analyzer.git
cd phishing_analyzer

# 2. Install package in editable mode with development tools
python -m pip install -e .[dev]
🚀 Quick Start & Usage
1. Launch Web GUI Dashboard
Start the local Flask application with automatic browser opening:

phishing-analyzer gui
(Or specify custom host/port: phishing-analyzer gui --host 0.0.0.0 --port 8080)

2. Command Line (CLI) Usage
Analyze a Single Email
# Terminal summary (default)
phishing-analyzer analyze samples/phish_cred_harvest.eml

# Export to standalone HTML report
phishing-analyzer analyze samples/phish_cred_harvest.eml --format html --output report.html

# Machine-readable JSON output
phishing-analyzer analyze samples/phish_cred_harvest.eml --format json
Batch Analyze Directory
phishing-analyzer batch samples/
🧪 Testing & Verification
The project enforces high test coverage and strict static analysis:

# Run pytest with coverage report
pytest --cov=phishing_analyzer
Test Results:

============================= 30 passed in 0.89s =============================
Coverage: 88% Total Coverage across all parser, check, scoring, and web modules.
🛠️ Technology Stack
Core Engine: Python 3.11+, email.policy.default, urllib.parse, hashlib
Scoring & Rules: Configurable YAML/JSON weights (config/default_rules.yaml)
Terminal UI: rich for formatted tables, panels, and colored risk indicators
Web Interface: Flask, Vanilla CSS (Custom Sepia & Espresso Theme), HTML5, JavaScript (ES6)
Testing: pytest, pytest-cov
🔒 Security & Privacy Commitments
100% Offline Static Inspection: No external API requests, DNS lookups, or remote image fetches occur during email analysis.
Defanged Output: All URLs (hxxp://), domains ([.]), and email addresses ([at]) are automatically defanged in reports to prevent accidental clicks.
No Code Execution: Attachments are inspected purely as static byte streams and SHA-256 hashes.
📜 License
Distributed under the MIT License. See LICENSE for more information.
