---
description: 'Ops, Automation, Shell Scripting, and Infrastructure Code Standards.'
applyTo: '**.ps1, **.sh, **.pp, **.yaml, **.yml'
---

# Ops & Automation Guidelines

## Your Mission
Create robust, idempotent, and secure infrastructure scripts. You ensure that operations are repeatable, traceable, and safe to execute in production environments.

## Introduction
Ops code is production code. It requires the same level of discipline as application code. This guide mandates idempotency, strict secret management, and cross-platform awareness.
**Critical Distinction:**
- **Agent:** Must **ALWAYS** use PowerShell for immediate system operations.
- **User:** Prefers **Bash/Shell** for generated scripts (`.sh`) and CI/CD pipelines.

## Fundamentals & Architecture

### Idempotency
- **Definition:** A script should be able to run multiple times without changing the result beyond the initial application.
- **Practice:** Always check state before mutating. "Check, then Act."

**Example - Idempotent One-Liner (PowerShell):**
```powershell
if (!(Test-Path $Path)) { New-Item -Path $Path -ItemType Directory }
```

### PowerShell (The Standard)
- **Strong Typing:** Use `[string]`, `[int]`, `[switch]` constraints.
- **CmdletBinding:** Always add `[CmdletBinding()]` to scripts/functions to enable `-Verbose`, `-WhatIf`.
- **Output:** Use streams correctly:
    - `Write-Verbose`: For diagnostics.
    - `Write-Warning`/`Error`: For issues.
    - `Write-Output` (implicit): For data to be piped.
    - ❌ `Write-Host`: Generally avoid, unless purely for UI status.

### Shell / Bash (User Preferred)
- **Context:** The user prefers Bash for their own scripts. When asked to generate a script, default to Bash unless specified otherwise.
- **Windows Compat:** Scripts must work in WSL or Git Bash.
- **Linting:** Must pass `SafeCheck`.
- **Shebang:** Always strictly define `#!/bin/bash`.

## Safety & Security

### Secret Management
- **The Golden Rule:** ❌ **NEVER** hardcode secrets (passwords, tokens, keys).
- **The Method:** ✅ Use Environment Variables (`$env:API_KEY`) or Secret Stores.
- **Logging:** Ensure logging does not leak secrets (e.g., avoid `Set-PSDebug -Trace 1` in production if it exposes vars).

### Input Validation
- Validate parameters using `ValidateSet`, `ValidateNotNullOrEmpty` attributes in PowerShell.

## Testing & Quality Assurance

### Validation
- **Simulations:** Use `-WhatIf` support in PowerShell scripts for dangerous operations.
- **Pre-flight Checks:** Verify prerequisites (admin rights, network access) before starting heavy work.

**Example - Robust PowerShell Function:**
```powershell
function New-SecureDir {
    [CmdletBinding(SupportsShouldProcess)]
    param (
        [Parameter(Mandatory)]
        [ValidateNotNullOrEmpty()]
        [string]$Path
    )

    if (-not (Test-Path $Path)) {
        if ($PSCmdlet.ShouldProcess($Path, "Create Directory")) {
            New-Item -ItemType Directory -Path $Path -Force | Out-Null
        }
    } else {
        Write-Verbose "Directory already exists: $Path"
    }
}
```

## Anti-patterns
- **Assumed Paths:** Hardcoding `C:\Temp` (it might not exist or be writable). Use `$env:TEMP`.
- **Blind Execution:** Running commands without checking `$LASTEXITCODE` or `$?` (in Bash/PS).
- **Zombie Processes:** Leaving background jobs running. Always clean up.
