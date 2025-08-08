<ins>Microsoft-Sentinel-Brute-Force-Detection-Automated-VM-Shutdown-Project</ins>

**Description**:
End-to-end Microsoft Sentinel automation detecting brute force attacks on Azure VMs and triggering instant shutdown via Logic Apps.  

1. <ins>Analytics Rule – Brute Force Detection</ins>    
<ins>MITRE ATT&CK Mapping</ins>  
-Tactic: Credential Access  
-Technique: T1110 - Brute Force  
-Sub-Technique: T1110.004 - Credential Stuffing  

**Query (KQL)**  
<img width="813" height="182" alt="AUTOMATION Rule Setting" src="https://github.com/user-attachments/assets/e934ce94-5e09-4834-b62a-ec02bf71760e" />  

Why: Event ID 4625 represents failed login attempts in Windows.  
How: This query counts failed attempts in a 5-minute window per username/computer.  
Effect: Detects brute force attempts early for rapid response.  

*Rule Settings**  
- Frequency: Every 5 minutes  
- Period: Last 5 minutes  
- Trigger: More than 0 results  
- Incident Creation: Enabled  
- Grouping: Match all entities in the last 5 minutes into one incident.  
[Azure_Sentinel_analytic_rule.json](https://github.com/user-attachments/files/21674647/Azure_Sentinel_analytic_rule.json)  
<img width="1734" height="867" alt="Analytic Rule Settings" src="https://github.com/user-attachments/assets/04067915-e833-4ac1-86b5-1050ebf1bb9f" />  

2. <ins>Automation – Logic App Playbook (PBK_BruteForce_Notify)</ins>  
Trigger: Microsoft Sentinel incident created from the above rule.  

**Actions:**  
-Send Emai1l (V2) – Notifies security team with subject: Brutal Force just occurred.  
-Power Off Virtual Machine – Stops the affected VM to prevent further intrusion.  

**Screenshots:**  
<img width="957" height="868" alt="Logic App Playbook" src="https://github.com/user-attachments/assets/6acf95e2-9ecf-49d2-8f9b-95755f4fb7c5" />  
<img width="1272" height="257" alt="Email Proof of Logic App" src="https://github.com/user-attachments/assets/22909dec-9369-47b8-9b42-31ac4b50c7dc" />  
<img width="946" height="534" alt="VM proof of Logic App" src="https://github.com/user-attachments/assets/df9fe16e-6e75-4843-af82-e9179c03f622" />  
<img width="960" height="458" alt="Logic App Run history" src="https://github.com/user-attachments/assets/7a0bc380-8a2c-4b99-955d-fa83a092ef6f" />  
Exported Logic App Template: [LogicAppExportedTemplate-NewTuesday.zip](https://github.com/user-attachments/files/21674679/LogicAppExportedTemplate-NewTuesday.zip)  

3. <ul>Automation Rule</ul>  
Name: BruteForce_Notify  
Trigger: When incident is created.  
Condition: Matches the current analytic rule name.  
Action: Run playbook PBK_BruteForce_Notify.    
<img width="813" height="182" alt="AUTOMATION Rule Setting" src="https://github.com/user-attachments/assets/744a16a5-06e4-47e0-ae45-eb786df78fe3" />  

4. <ul>Execution & Validation</ul>  
-Manual brute force simulation triggered Event ID 4625 logs.  
-Sentinel analytic rule detected 3+ failed attempts within 5 minutes.  
-Incident generated automatically.  
-Automation rule triggered the Logic App.  
-Email sent to SOC mailbox.  
-VM was powered off within seconds.  

Proof Screenshots:  
-Detection: <img width="1734" height="867" alt="Analytic Rule Settings" src="https://github.com/user-attachments/assets/0350393d-8f20-43dd-be68-4427b82fc3ef" />  

-Email Alert:  <img width="1272" height="257" alt="Email Proof of Logic App" src="https://github.com/user-attachments/assets/7619af91-fe51-4f16-95de-6a3e2ab20db5" />  

-VM Shutdown:  <img width="946" height="534" alt="VM proof of Logic App" src="https://github.com/user-attachments/assets/e229f45b-8f83-4824-a3dc-6735bfba66a9" />  

-Automation Run:    <img width="960" height="458" alt="Logic App Run history" src="https://github.com/user-attachments/assets/70a36e7e-8715-4e81-9bb6-e5e6facc44e4" />  

5. <ul>Why This Matters</ul>  
What it solves: Stops brute force attacks before account takeover.  
Why it works: Links detection (KQL analytics) directly to automated containment (Logic Apps).  
Impact: Immediate mitigation without waiting for human intervention.  
Best Use Case: High-value VMs with exposed RDP or public endpoints.  

6. <ul>Proof of Incident in Sentinel</ul>  
<img width="1916" height="880" alt="Proof of incident in Sentinel" src="https://github.com/user-attachments/assets/23caa9b6-010e-4bb7-9feb-3df815342543" />  
Screenshot showing the “Brute Force Attack” incident created automatically in Microsoft Sentinel after the simulated attack.  

7. <ul>Files</ul>  


[Azure_Sentinel_analytic_rule.json](https://github.com/user-attachments/files/21674793/Azure_Sentinel_analytic_rule.json): Full Sentinel detection rule  
- [LogicAppExportedTemplate-NewTuesday.zip](https://github.com/user-attachments/files/21674796/LogicAppExportedTemplate-NewTuesday.zip) : Full Logic App export  
- <img width="1734" height="867" alt="Analytic Rule Settings" src="https://github.com/user-attachments/assets/0c46ae01-4a40-4ccf-928c-32c4f84efc79" />  
- <img width="813" height="182" alt="AUTOMATION Rule Setting" src="https://github.com/user-attachments/assets/4fa5d464-b16d-431b-b2fe-653e69ef79a1" />  
- <img width="1272" height="257" alt="Email Proof of Logic App" src="https://github.com/user-attachments/assets/4787dea3-08f4-44cb-b361-fdc29b39b3de" />  
- <img width="957" height="868" alt="Logic App Playbook" src="https://github.com/user-attachments/assets/b62be946-150b-4004-8c57-2589a50866c4" />  
- <img width="960" height="458" alt="Logic App Run history" src="https://github.com/user-attachments/assets/7dec6ad9-48df-4ae8-8939-a334e1f6c6c5" />
- <img width="946" height="534" alt="VM proof of Logic App" src="https://github.com/user-attachments/assets/b40d3ac7-5352-4c55-8025-43c10e9a5e29" />  

