# AndyWeSec-FUTURE_CS_02
Repository for the Cyber Security Internship under the Fellowship Program at Future Interns (August 2026 – September 2026).Task 2: Phishing Email Analysis

# Vulnerability Assessment Report: Phishing Email Analysis

**Date of Assessment:** *August 31, 2026*  
**Tools Used:** MXToolbox Header Analyzer  
**Classification:** 🔴 **Phishing — High Risk**

---

## 📋 Executive Summary
An email purporting to be from an internal "IT Service Desk" was submitted for analysis and confirmed to be a phishing attempt. The message combines technical spoofing techniques — including a typosquatted sender domain and an insecure malicious link — with classic social engineering tactics such as urgency and impersonation. The email should be treated as malicious, reported, and blocked at the mail gateway level.

---

## 🚨 Detailed Findings & Risk Breakdown

### 1. Spoofed Sender Address
* **What is the issue?** The display name claims to be the official "IT Service Desk," but header analysis (via MXToolbox Header Analyzer) reveals the true sender address as `security-update@micros0ft-support.com` — with the letter "o" replaced by the number "0".
* **Why does it matter?** This is a deliberate typosquatting technique designed to visually resemble a legitimate Microsoft domain, exploiting the tendency of users to trust a familiar display name without checking the underlying address. It is specifically engineered to bypass casual visual inspection.
* **Risk Level:** 🔴 **High**
* **Remediation:** Configure email security gateways to flag or block domains using homoglyph/character-substitution patterns targeting trusted brand names. Train staff to inspect the actual sender address, not just the display name.
  <img width="1334" height="359" alt="Screenshot 2026-08-31 at 10 09 16" src="https://github.com/user-attachments/assets/a1d611a6-9c22-40b0-ba0f-a27f8897ac7c" />
  *MXToolbox Header Analyzer output confirming the true sender address (`security-update@micros0ft-support.com`) behind the "IT Service Desk" display name — note the zero substituted for the letter "o".*

### 2. Insecure URL Protocol
* **What is the issue?** The embedded hyperlink uses unencrypted `http://` (`http://login-microsoft-secure-portal.com`) rather than secure `https://`.
* **Why does it matter?** Legitimate corporate login portals — especially those belonging to major providers like Microsoft — universally use HTTPS. The use of plain HTTP is a strong technical red flag and indicates the link was not built through legitimate infrastructure.
* **Risk Level:** 🔴 **High**
* **Remediation:** Implement email filtering rules that flag outbound links using insecure protocols, and reinforce user training on checking for HTTPS before submitting any credentials.

### 3. Malicious Destination Site
* **What is the issue?** The link redirects to a page designed to harvest sensitive information — including passwords, phone numbers, and credit card details — or to trick visitors into installing malicious software.
* **Why does it matter?** Modern browsers (e.g. Google Chrome) already flag this specific domain as unsafe and display a warning page, independently corroborating that the destination is a known malicious site rather than a false positive.
* **Risk Level:** 🔴 **High**
* **Remediation:** Block the domain at the DNS/firewall level, submit it to threat intelligence feeds (e.g. Google Safe Browsing, PhishTank) if not already flagged, and ensure endpoint protection is active organization-wide.
<img width="1162" height="756" alt="Screenshot 2026-08-31 at 10 18 20" src="https://github.com/user-attachments/assets/4d9ebe68-c890-4524-a959-6bfce3d68874" />
*Google Chrome's built-in Safe Browsing protection flagging `login-microsoft-secure-portal.com` as a dangerous site, independently corroborating the phishing link's malicious destination.*

### 4. Generic Greeting
* **What is the issue?** The email opens with "Dear Customer" rather than addressing the recipient by name.
* **Why does it matter?** Legitimate internal IT communications typically reference the employee by name or account-specific details. A generic greeting suggests a mass-distributed phishing campaign rather than a targeted, authentic internal message.
* **Risk Level:** 🟠 **Medium**
* **Remediation:** Encourage staff to treat generically-addressed "urgent" emails from internal departments with suspicion, and verify through a separate communication channel (e.g. phone, Slack) before acting.

### 5. Artificial Urgency
* **What is the issue?** The email threatens permanent account suspension within a strict 2-hour window.
* **Why does it matter?** This is a classic psychological pressure tactic used to provoke hasty action and bypass rational scrutiny, a hallmark of social engineering attacks designed to short-circuit normal verification habits.
* **Risk Level:** 🟠 **Medium**
* **Remediation:** Train staff to recognize artificial time-pressure tactics as a phishing indicator, and establish a "cool-down" policy — any account-security email demanding immediate action should be independently verified before compliance.

### 6. Spelling and Grammar Errors
* **What is the issue?** The email body contains grammatical errors, including a misspelling of the word "corporate."
* **Why does it matter?** Poor spelling and grammar remain a common — if increasingly inconsistent — indicator of phishing, often resulting from mass-produced templates, translation artifacts, or non-native authorship by threat actors.
* **Risk Level:** 🟡 **Low**
* **Remediation:** Include spelling/grammar inconsistencies as one signal (not a sole determinant) in phishing-awareness training, alongside the stronger technical indicators above.

---

## 🛡️ Recommended Mitigation & Action Steps

1. **Verify Senders:** Never rely on display names alone — inspect the exact domain string for deceptive character substitutions (e.g. `0` for `o`).
2. **Inspect Before Interacting:** Always hover over hyperlinks to check the destination domain and confirm the protocol is `https://` before clicking.
3. **Report Suspicious Activity:** Do not reply to the sender or enter any credentials. Forward flagged messages directly to the internal IT security mailbox for isolation and analysis.
4. **Block at Source:** Add the sender domain and malicious link to the organization's email/DNS blocklists to prevent further delivery.

---

## 🛠️ Summary Action Roadmap

1. **Immediate:** Block the sender domain (`micros0ft-support.com`) and malicious link domain at the email gateway and DNS level; report the sample to internal security and external threat intelligence feeds.
2. **Short Term:** Notify any recipients who may have received the email and confirm none have submitted credentials to the phishing site.
3. **Ongoing:** Reinforce phishing-awareness training focused on sender-domain inspection, HTTPS verification, and recognizing urgency-based social engineering tactics.

---
