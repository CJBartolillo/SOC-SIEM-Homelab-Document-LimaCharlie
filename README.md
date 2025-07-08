# SOC Analyst Homelab - Cyber Attack Detection & Response

This homelab demonstrates a complete SOC environment simulating real-world cyber attacks and defensive responses. Following Eric Capuano's guide "So you want to be a SOC Analyst?", this project showcases threat detection, incident response, and security monitoring using industry-standard tools.

**Reference Guide:** [So you want to be a SOC Analyst? - Eric Capuano](https://blog.ecapuano.com/p/so-you-want-to-be-a-soc-analyst-intro)

## Lab Setup

The lab consists of two virtual machines: an Ubuntu Server acting as the attack machine running Sliver C2 framework, and a Windows 11 endpoint as the victim machine with LimaCharlie EDR installed. Windows Defender and other security features were disabled to allow for realistic attack simulation. LimaCharlie sensors were configured to monitor the Windows endpoint and import Sysmon logs for comprehensive telemetry collection.

Windows 11 machine:
![image](https://github.com/user-attachments/assets/cb742db2-94a7-442a-be25-8f959c0d35d6)
![image](https://github.com/user-attachments/assets/b6795ea1-0d08-4bde-a427-e2a4ebcf93df)


Ubuntu Server machine: 
![image](https://github.com/user-attachments/assets/900c80bb-128c-4aad-b31d-7864598f0308)




## Phase 1: Payload Generation and C2 Establishment

The first step involved generating a malicious payload using Sliver and deploying it to the Windows endpoint. Once executed, the payload established a command and control session between the attack machine and victim, allowing for remote access and system enumeration.

Using SSH to access the Ubuntu server to download and launch Sliver to begin the process of delivering the payload.
![image](https://github.com/user-attachments/assets/e27f3c05-568c-40b7-8fec-2216e4fe9bb3)
![image](https://github.com/user-attachments/assets/cca6d055-ff7b-4c25-8809-afe23d941328)
Verifying we have established a session between the two machines after the Windows machine downloaded the payload via a python3 http webserver.
![image](https://github.com/user-attachments/assets/b60e575e-fe56-471b-9add-e722e01b5c16)
Running commands to simulate attacker poking around for information
![image](https://github.com/user-attachments/assets/d69d2ce1-3bc1-45e0-9de0-40bbcd4845cf)
ps -T command which Sliver highlights any defensive processes in red and its own process in green.
![image](https://github.com/user-attachments/assets/cbb0395c-07b5-4243-b2b0-a9ed7f898867)








## Phase 2: Attack Simulation and Detection

With an active C2 session established, various attack techniques were executed including privilege escalation attempts, system reconnaissance, and process enumeration. LimaCharlie provided real-time telemetry showing the attacker's activities, IP connections, and running processes on the endpoint.

![Attack Detection](screenshots/attack-detection.png)

## Phase 3: Hash Analysis and Threat Intelligence

The malicious payload was analyzed through LimaCharlie's VirusTotal integration to demonstrate hash-based detection capabilities. Since the payload was custom-generated, it appeared clean in threat intelligence feeds, highlighting the importance of behavioral detection over signature-based approaches.

![Hash Analysis](screenshots/hash-analysis.png)

## Phase 4: Credential Harvesting Simulation

Advanced attack techniques were simulated, including LSASS memory dumping to extract credentials. LimaCharlie detected these sensitive process access attempts, and custom detection rules were written to identify and alert on credential theft activities.

![Credential Harvesting](screenshots/credential-harvesting.png)

## Phase 5: Detection Rule Development and Response

Beyond simple detection, proactive response capabilities were implemented. Custom detection rules were created to identify and automatically block malicious activities. Ransomware simulation was performed by attempting to delete volume shadow copies, with LimaCharlie successfully detecting and blocking the attack through automated response rules.

![Automated Response](screenshots/automated-response.png)

## Tools Used

- **LimaCharlie EDR:** Endpoint detection and response platform
- **Sliver C2:** Command and control framework for attack simulation
- **Sysmon:** Windows system monitoring and logging
- **Ubuntu Server:** Attack platform
- **Windows 11:** Target endpoint
- **VirusTotal:** Malware analysis and threat intelligence

## Conclusion and Key Learnings

This SOC homelab successfully demonstrated end-to-end security operations from threat detection through automated response. The project provided hands-on experience with industry-standard tools and realistic attack scenarios.

**Key technical skills developed:**
- SIEM configuration and custom detection rule creation
- Threat hunting and behavioral analysis techniques
- Incident response and automated threat containment
- Malware analysis and threat intelligence integration

**Critical insights gained:**
- Behavioral detection is more effective than signature-based approaches for unknown threats
- Proper alert tuning is essential to reduce false positives while maintaining detection coverage
- Automated response capabilities significantly improve threat containment speed
- Comprehensive logging and telemetry collection are fundamental to effective threat detection

This practical experience demonstrates readiness for SOC analyst responsibilities and provides a strong foundation for advanced security operations roles.

---

*This project was completed in an isolated lab environment following ethical guidelines and best practices.*
