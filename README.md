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









## Phase 2: Attack Simulation and Detection

With an active C2 session established, various attack techniques were executed including privilege escalation attempts, system reconnaissance, and process enumeration. LimaCharlie provided real-time telemetry showing the attacker's activities, IP connections, and running processes on the endpoint.

Running commands to simulate attacker poking around for information
![image](https://github.com/user-attachments/assets/d69d2ce1-3bc1-45e0-9de0-40bbcd4845cf)
ps -T command which Sliver highlights any defensive processes in red and its own process in green.
![image](https://github.com/user-attachments/assets/cbb0395c-07b5-4243-b2b0-a9ed7f898867)

Heading to our LimaCharlie app we can see some suspicious unsigned processes within our sensor.
![telemetry](https://github.com/user-attachments/assets/d6c6885a-0b73-4f33-bdb1-bbd31429643b)
Scrolling further we can find another unsigned .exe process
![image](https://github.com/user-attachments/assets/103c9de8-e610-4aa5-981a-af0150e17875)

Viewing the connection reveals the attackers IP
![image](https://github.com/user-attachments/assets/1386574d-030a-441c-8e98-50232531659e)

Timeline of events in LimaCharlie viewing the suspected IP Address from the attacker
![image](https://github.com/user-attachments/assets/41b70e0b-249b-4d58-8e02-438ea9d90e08)
Network tab showcasing established connection to IP
![image](https://github.com/user-attachments/assets/86794cbf-32b5-4593-8e4f-efa1de0e2e42)
File Tab reveals the file and its location
![image](https://github.com/user-attachments/assets/e7719b9c-51c6-488a-ae04-f916962a695e)









## Phase 3: Hash Analysis and Threat Intelligence

The malicious payload was analyzed through LimaCharlie's VirusTotal integration to demonstrate hash-based detection capabilities. Since the payload was custom-generated, it appeared clean in threat intelligence feeds, highlighting the importance of behavioral detection over signature-based approaches.

Inspecting the file hash through VirusTotal reveals no results found, which to me signals an even bigger indicator that this is a malicious software
![image](https://github.com/user-attachments/assets/95c8cbe0-ceb2-4d24-888f-7abb8452dd60)


## Phase 4: Credential Harvesting Simulation

Advanced attack techniques were simulated, including LSASS memory dumping to extract credentials. LimaCharlie detected these sensitive process access attempts, and custom detection rules were written to identify and alert on credential theft activities.

Credential dumping
![image](https://github.com/user-attachments/assets/b7e2b6fc-abaf-4444-819c-b7a199c29b3f)



## Phase 5: Detection Rule Development and Response

Beyond simple detection, proactive response capabilities were implemented. Custom detection rules were created to identify and automatically block malicious activities. Ransomware simulation was performed by attempting to delete volume shadow copies, with LimaCharlie successfully detecting and blocking the attack through automated response rules.

Creating a Detection and Response rule for this specific Lsass attack
![image](https://github.com/user-attachments/assets/668b284c-f99e-417c-964b-bc0e98af8915)

After running a second lsass dump command, our detection rule has picked up and alerted us about it.
![image](https://github.com/user-attachments/assets/32879607-246f-4568-b6a7-750598f3f759)
Creating telemetry for deleting shadow copies, a common ransomware method. We are going to create a blocking rule to try prevent this in the future.
![image](https://github.com/user-attachments/assets/e9124598-3a4a-4b87-81bf-d155e03edbbf)
creating detection rule based off a sigma detection signature, added the response section to hopefully block and prevent this adverserial tactic in the future
![image](https://github.com/user-attachments/assets/6cd31532-5404-43ce-adad-31e19d7fc937)
the D&R rule worked by showing the practical result - the LSASS command failed and the shell exited, proving the automated blocking was successful.
![image](https://github.com/user-attachments/assets/c7970963-4506-4d30-b44e-47b049a9ec22)












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
