# Analysis – CAR-2013-09-003 (SMB Session Setups)

## 1. Questions I Wanted to Answer

- What does a normal SMB session setup look like in my lab?
- How do SMB session setups differ during scanning/lateral movement?
- Which fields are most useful for detection (e.g., username, hostname, IP, frequency)?
- How noisy is this analytic in a typical environment?

---

## 2. Network-Level Observations

### 2.1 Normal SMB Usage

- **Source:** Windows 10 → **Destination:** Windows Server
- Typical pattern:
  - A small number of connections from one client to one server.
  - SMB session setup, tree connect, file operations, then teardown.
- Observed fields:
  - Source IP: `...`
  - Dest IP: `...`
  - Dest port: 445
  - Auth user: `labuser1`

### 2.2 Suspicious / Scan-like Behavior

- **Source:** Kali → multiple hosts
- Pattern:
  - Many SMB session setup attempts in quick succession across multiple IPs.
  - Some failed auth, some succeeded.
- Signs of lateral movement / reconnaissance:
  - High fan-out (1 → many hosts).
  - Repeated authentication attempts by same user to many targets.

---

## 3. Zeek / Log Observations

*(If applicable)*

- In `smb_cmd.log`, session setup commands appeared as:
  - Command: `SESSION_SETUP_ANDX` or similar.
  - Username fields: `labuser1`, `anonymous`, etc.

- In `conn.log`, notable:
  - High number of short-lived connections from single source.
  - Spike during scanning activity.

---

## 4. Host-Level Observations

Sysmon & Security Logs:

- Event IDs observed:
  - e.g. Security 4624 (logon)
  - Failed logons (4625)
- Correlation between:
  - SMB session setup network events
  - Logon events on the server

---

## 5. Detection Logic Rationale

The detection focuses on:

- **Abnormal fan-out:** one source contacting many SMB servers in a short time window.
- **Unusual authentication patterns:** repeated logons to multiple hosts by same user.
- **Timing patterns:** bursty SMB session setups over a short period.

These behaviors are rare in normal user activity but common during scanning or lateral movement.

---

## 6. Limitations & False Positives

Potential false positives:

- Backup servers scanning many hosts via SMB.
- Security tools or inventory scanners using SMB.
- Admin scripts that iterate through hosts to push updates.

Mitigation:

- Whitelist known scanner/backup/service accounts.
- Add thresholds (e.g. N hosts in M minutes).
- Scope to specific subnets or privileged accounts.

---

## 7. Future Improvements

- Add user context enrichment (e.g. is this an admin? service account?).
- Correlate with process info if available (e.g. which process initiated SMB connection).
- Integrate with a SIEM and alert only when multiple conditions are met (user, fan-out, time window).

