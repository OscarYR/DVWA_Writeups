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
| [Brute Force](./1.%20Brute%20Force/Brute%20Force.md) | Low / Medium / High | ✅ Completed |
| [Command Execution](./2.%20Command%20Execution/Command%20Execution.md)  | Low / Medium / High | ✅ Completed |
| [Cross Site Request Forgery](./3.%20Cross%20Site%20Request%20Forgery/Cross%20Site%20Request%20Forgery.md)            | Low / Medium / High | ✅ Completed |
| [File Inclusion](./4.%20File%20Inclusion/File%20Inclusion.md) | Low / Medium / High | ✅ Completed |
| [File Upload](./5.%20File%20Upload/File%20Upload.md) | Low / Medium / High |  ✅ Completed |
| [Insecure CAPTCHA](./6.%20Insecure%20CAPTCHA/Insecure%20CAPTCHA.md) | Low / Medium / High |  ✅ Completed |
| [SQL Injection](./7.%20SQL%20Injection/SQL%20Injection.md) | Low / Medium / High |  ✅ Completed |

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
