# DVWA Web Application Write-Ups

This repository contains a series of write-ups for [DVWA (Damn Vulnerable Web Application)](https://github.com/digininja/DVWA), a purposefully insecure web application used to practice and learn various web security vulnerabilities.

These walkthroughs were done in a lab environment using:
- 🖥️ **Attacker**: Kali Linux
- 🧪 **Victim**: DVWA in Metasploitable 2
- 🔧 **Tools**: Burp Suite, Hydra, curl, Firefox Developer Tools

---

## 💡 Covered Vulnerabilities

Each folder contains:
- Step-by-step exploitation
- Tool usage (Burp Suite, manual, etc.)
- Screenshots (if applicable)
- Observations across **Low**, **Medium**, and **High** difficulty levels
- Skills and techniques learned

### 📁 Write-Ups So Far
| Challenges        | Difficulty | Status  |
|----------------|------------|---------|
| [Brute Force](./01.%20Brute%20Force/Brute%20Force.md) | Low / Medium / High | ✅ Completed |
| [Command Execution](./02.%20Command%20Execution/Command%20Execution.md)  | Low / Medium / High | ✅ Completed |
| [Cross Site Request Forgery](./03.%20Cross%20Site%20Request%20Forgery/Cross%20Site%20Request%20Forgery.md)            | Low / Medium / High | ✅ Completed |
| [File Inclusion](./04.%20File%20Inclusion/File%20Inclusion.md) | Low / Medium / High | ✅ Completed |
| [File Upload](./05.%20File%20Upload/File%20Upload.md) | Low / Medium / High |  ✅ Completed |
| [Insecure CAPTCHA](./06.%20Insecure%20CAPTCHA/Insecure%20CAPTCHA.md) | Low / Medium / High |  ✅ Completed |
| [SQL Injection](./07.%20SQL%20Injection/SQL%20Injection.md) | Low / Medium / High |  ✅ Completed |
| [Blind SQL Injection](./08.%20Blind%20SQL%20Injection/Blind%20SQL%20Injection.md) | Low / Medium / High |  ✅ Completed |
| [Weak Session IDs](./09.%20Weak%20Session%20IDs/Weak%20Session%20IDs.md) | Low / Medium / High |  ✅ Completed |
| [DOM XSS](./10.%20DOM%20XSS/DOM%20XSS.md) | Low / Medium / High |  ✅ Completed |
| [Reflected XSS](./11.%20Reflected%20XSS/Reflected%20XSS.md) | Low / Medium / High |  ✅ Completed |
| [Stored XSS](./12.%20Stored%20XSS/Stored%20XSS.md) | Low / Medium / High |  ✅ Completed |
| [CSP Bypass](./13.%20CSP%20Bypass/CSP%20Bypass.md) | Low / Medium / High |  ✅ Completed |
| [JavaScript](./14.%20JavaScript/JavaScript.md) | Low / Medium / High |  ✅ Completed |
| [Authentication Bypass](./15.%20Authentication%20Bypass/Authentication%20Bypass.md) | Low / Medium / High |  ✅ Completed |

---

## 🧠 Skills Learned

- Web vulnerability identification and exploitation
- Use of **Burp Suite Intruder** for brute forcing
- Analysis of HTTP request/response lengths and keyword patterns
- Credential harvesting through response analysis
- Understanding impact of difficulty levels (filtering, delays, etc.)

---

## 📌 Notes

- All activities were performed in a **safe lab environment**
- Educational purposes only
