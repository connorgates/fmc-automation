Cisco FMC Automation ToolkitA lightweight, interactive CLI toolkit for managing and auditing Cisco Secure Firewall Management Center (FMC) via REST API. Designed for Network Engineers and InfoSec analysts to perform rapid policy searches, object management, rule audits, and instant dynamic IP blocking without full policy deployments.Key FeaturesInstant Dynamic Blocking (InfoSec Operations): Block or unblock malicious IPs instantly using FMC Dynamic Objects—taking effect across FTD firewalls in real-time without requiring a policy deployment.Access Control Rule Search & Audit: Search policies by keyword, port, or IP, and run parity audits across policy pairs.Object Management: Audit unused network/service objects and dump full object definitions to CSV/console.Automated Rule Creation: Bulk ingest access rules from structured CSV files.Global CLI Launcher: Run the menu instantly from anywhere on Windows using a single terminal command (fmc) or the Win + R Run dialog.RequirementsOS: Windows 10 / 11 / Windows ServerEnvironment: PowerShell 5.1+ or PowerShell Core (pwsh)Python: Python 3.10 or higherPermissions: Cisco FMC user account with REST API access privilegesGetting Started (First-Time Setup)Follow these steps when cloning or downloading this project onto a new machine.1. Clone or Download the RepositoryOpen PowerShell and clone the repo (or extract the downloaded ZIP file):git clone https://github.com/YOUR_ORGANIZATION/fmc-automation.git
cd fmc-automation
2. Set Up Python Virtual Environment & DependenciesCreate a virtual environment, activate it, and install the required packages:# Create venv
python -m venv venv

# Activate venv
.\venv\Scripts\Activate.ps1

# Install requirements
pip install requests python-dotenv
3. Configure API CredentialsCreate a .env file in the root folder of the project with your FMC details:FMC_HOST=10.0.0.50
FMC_USER=api_user
FMC_PASS=YourSecretPasswordHere
Note: Never commit the .env file to source control. Ensure .env is listed in your .gitignore file.🚀 One-Click Global Command Setup (fmc)To allow anyone on your team to type fmc in terminal or press Win + R -> fmc from anywhere on Windows, run this one-line PowerShell setup script inside the project folder:# Run this once inside the fmc-automation project folder:
$projPath = Get-Location; Set-Content -Path "$projPath\fmc.bat" -Value "@echo off`r`ncd /d `"%~dp0`"`r`npowershell -ExecutionPolicy Bypass -File `"menu.ps1`""; [Environment]::SetEnvironmentVariable("Path", $env:Path + ";$projPath", "User"); Write-Host "[+] Setup Complete! Close and reopen your terminal, then type 'fmc' from anywhere." -ForegroundColor Green
How to Launch the Toolkit:After running the command above, close and reopen your terminal or press Win + R:Option A (Terminal): Open CMD or PowerShell anywhere and type:fmc
Option B (Run Dialog): Press Win + R, type fmc, and hit Enter.Toolkit Menu Breakdown==========================================
        FMC AUTOMATION TOOLKIT
==========================================
 [1] Test FMC Connection
 [2] Search / Dump Access Control Rules
 [3] Audit IPs in Rules (Edit in Script)
 [4] Export FMC Devices
 [5] Find Unused Objects
 [6] Get Full Objects
 [7] Create Rule from CSV
 [8] Block/Unblock/View Malicious IP (Dynamic Object)
 [Q] Quit
==========================================
OptionNameDescription1Test FMC ConnectionVerifies credentials and test-authenticates with the FMC REST API.2Search / Dump RulesInteractively search across policies for rules matching keywords, IP addresses, or ports.3Audit IPs in RulesAudits parity between target host/subnet configurations across policy sets.4Export FMC DevicesDumps registered FTD devices, HA pairs, and software versions to CSV/screen.5Find Unused ObjectsScans FMC for network/host objects not referenced by any active rules.6Get Full ObjectsDumps detailed configurations for all Network and Service Objects.7Create Rule from CSVBulk creates Access Rules from new_rule_layout.csv.8Dynamic Object ManagerInteractive menu to Add (Block), Remove (Unblock), or View IPs on FMC Dynamic Objects without policy deployment delay.Project Structurefmc-automation/
├── .env                         # API Credentials (local only)
├── .gitignore                   # Ignores .env and venv
├── fmc.bat                      # Windows CMD wrapper for global command execution
├── menu.ps1                     # Interactive PowerShell Launcher
├── ops_dynamic_objects.py       # Instant dynamic blocklist manager
├── test_conn.py                 # FMC REST API auth test script
├── search_rules.py              # Policy rule search engine
├── check_ip_rules.py            # Rule audit & parity verification
├── export_devices.py            # Device inventory exporter
├── find_unused_objects.py       # Unused object audit tool
├── get_full_objects.py          # Object definition exporter
├── create_rule.py               # CSV rule creation engine
└── README.md                    # Project documentation
Security NotesRead-Only Operations: Scripts 1 through 6 are read-only and do not alter firewall policy configurations.Surgical Operations: Option 8 only modifies IP address mappings within designated Dynamic Objects (InfoSec_Blocklist), preserving policy integrity.SSL Warnings: REST API calls disable self-signed certificate warnings (urllib3.disable_warnings) for internal network compatibility.
