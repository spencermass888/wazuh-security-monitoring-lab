# wazuh-security-monitoring-lab
Wazuh lab covering Windows endpoint monitoring, authentication events, account and group changes, and real-time file integrity monitoring.

# Wazuh Security Monitoring Lab

This lab demonstrates deploying Wazuh on Ubuntu, connecting a Windows endpoint, investigating security events, and monitoring file changes.

## Part 1: Wazuh Server Deployment and Verification

### 1. Completing Installation and Checking the Manager

<img width="1058" height="715" alt="image" src="https://github.com/user-attachments/assets/2fec2d07-f90b-42ca-a52e-a26b49ec3f37" />

I completed the Wazuh installation and checked the manager service, confirming that it was active and running.

### 2. Verifying the Core Services

<img width="1052" height="302" alt="image" src="https://github.com/user-attachments/assets/d79d6775-36f7-4b16-be94-f2b4d0656823" />

I verified that the Wazuh manager, indexer, dashboard, and Filebeat services were all active.

### 3. Checking the Dashboard Service

<img width="1004" height="563" alt="image" src="https://github.com/user-attachments/assets/0d0b1cbd-e050-4f82-a9fa-7624b4532e1c" />

I checked the Wazuh dashboard service separately after signing in to the Ubuntu server and confirmed that it was active.

### 4. Configuring Dashboard Port Forwarding

<img width="1012" height="536" alt="image" src="https://github.com/user-attachments/assets/daf77716-a058-4832-afa2-063393b87aed" />

I configured VirtualBox NAT port forwarding from local TCP port 8443 to the Wazuh server’s HTTPS port 443, allowing dashboard access from the host computer.

### 5. Accessing the Wazuh Dashboard

<img width="1054" height="471" alt="image" src="https://github.com/user-attachments/assets/9652b71c-5de6-4d97-9b23-f9f6761e141b" />

I opened the Wazuh dashboard through the forwarded local address and verified that the interface loaded. No endpoint agents had been registered at this stage.

### Part 1 Summary

I deployed Wazuh on an Ubuntu virtual machine, verified its core services, and configured dashboard access through VirtualBox port forwarding. This established the monitoring platform for the Windows endpoint work that followed.

## Part 2: Windows Endpoint Deployment and Security Event Investigation

### 1. Configuring the Windows Endpoint Network

<img width="1021" height="436" alt="image" src="https://github.com/user-attachments/assets/4254d7c4-6f3e-4acc-830d-69e4f08d58ff" />

I connected the WIN10-ENDPOINT virtual machine to the Wazuh-NAT network in VirtualBox to prepare it for communication with the Wazuh server.

### 2. Configuring the Wazuh Server Network

<img width="1033" height="491" alt="image" src="https://github.com/user-attachments/assets/b1aa5297-bcfa-40ec-84bc-224df71aef81" />

I confirmed that the Ubuntu Wazuh server was connected to the same Wazuh-NAT network as the Windows endpoint.

### 3. Verifying the Wazuh Server IP Address

<img width="1048" height="591" alt="image" src="https://github.com/user-attachments/assets/a5cc595f-84a0-4a19-b50c-8e5b953f8eec" />

I ran ip addr on the Ubuntu server and identified 10.0.2.3 as the server address used during this part of the lab.

### 4. Testing Connectivity from Windows

<img width="1070" height="801" alt="image" src="https://github.com/user-attachments/assets/df1bf35a-e9f7-45bd-b25c-8794e83a0188" />

I successfully pinged 10.0.2.3 from the Windows endpoint and used Test-NetConnection to verify connectivity to TCP port 1514.

### 5. Configuring Windows Agent Deployment

<img width="1026" height="552" alt="image" src="https://github.com/user-attachments/assets/fc983263-7fcc-4f8f-b6b2-d477ad5af282" />

I selected the Windows agent package in the Wazuh dashboard, entered the manager address 10.0.2.3, and assigned the agent name WIN10-ENDPOINT.

### 6. Verifying the Windows Agent Service

<img width="1063" height="305" alt="image" src="https://github.com/user-attachments/assets/809f097a-7d97-4825-a87e-aa2786eb9d23" />

I checked WazuhSvc in PowerShell and confirmed that the Windows agent service was running.

### 7. Generating a Failed Sign-in Attempt

<img width="1046" height="348" alt="image" src="https://github.com/user-attachments/assets/15255099-7161-4d50-b730-7385f0ce7fb7" />

I attempted to launch a command prompt using the LabUser account. Windows rejected the supplied credentials, generating authentication activity to investigate.

### 8. Confirming an Active Agent

<img width="1004" height="433" alt="image" src="https://github.com/user-attachments/assets/f475b41f-5743-4467-adc1-b220e3f78aa2" />

I checked the Wazuh dashboard and confirmed that it reported one active agent and no disconnected agents.

### 9. Reviewing Endpoint Registration

<img width="1000" height="428" alt="image" src="https://github.com/user-attachments/assets/c2fb6bbe-5e3a-45a2-8813-75c10527190b" />

I verified that WIN10-ENDPOINT appeared as agent 001, running Windows 10 Pro with an active status.

### 10. Reviewing Endpoint Security Information

<img width="1055" height="370" alt="image" src="https://github.com/user-attachments/assets/a534dc07-6858-43e4-965b-c252f23d7a19" />

I reviewed the endpoint’s vulnerability detection and security configuration assessment panels. No recent File Integrity Monitoring events were displayed at this stage.

### 11. Investigating Failed Sign-in Event Details

<img width="990" height="1044" alt="image" src="https://github.com/user-attachments/assets/634c2602-1ca0-489c-a48a-d37b8f752ba5" />

I examined a failed sign-in alert’s Document Details. The record showed Windows Security event ID 4625 and Wazuh rule 60122 for an unknown user or bad password.

### Part 2 Summary

I connected the Windows endpoint and Ubuntu server to the same virtual network, verified connectivity, and configured the Windows agent. I confirmed that the endpoint was active in Wazuh, generated a failed sign-in attempt, and investigated the recorded authentication event.

## Part 3: Monitoring Windows Account and Group Changes

### 1. Reviewing Windows Security Events

<img width="975" height="880" alt="image" src="https://github.com/user-attachments/assets/8fcffe2e-916c-4aa4-a84a-bbbbd994e3c8" />

I used Wazuh Threat Hunting to review events from WIN10-ENDPOINT, including failed sign-in alerts.

### 2. Examining a Failed Sign-in Alert

<img width="900" height="892" alt="image" src="https://github.com/user-attachments/assets/bddcbded-4845-4076-97e5-42ec1e8af87c" />

I opened the alert’s Document Details and identified an audit failure under Wazuh rule 60122, described as an unknown user or bad password.

### 3. Creating a Test User Account

<img width="900" height="499" alt="image" src="https://github.com/user-attachments/assets/f301b32c-42e9-4b83-b0a6-63e215d66154" />

I created a temporary local Windows account named WazuhLabUser using PowerShell. Windows confirmed that the command completed successfully.

### 4. Reviewing Account Creation Alerts

<img width="986" height="524" alt="image" src="https://github.com/user-attachments/assets/5acf1d8a-ab51-4e2f-95ab-362c324d19b0" />

I reviewed the new Wazuh alerts for user account creation and related group changes on the Windows endpoint.

### 5. Investigating Account Creation Details

<img width="900" height="895" alt="image" src="https://github.com/user-attachments/assets/0b7b5e8b-ad6e-45f0-8d50-54aeadcf3df7" />

I examined an account creation or enablement alert under Wazuh rule 60109, level 8. The alert included a MITRE ATT&CK mapping to Account Manipulation.

### 6. Adding the Test User to Administrators

<img width="1023" height="322" alt="image" src="https://github.com/user-attachments/assets/a9f31982-a6d0-4840-ac23-5d249378df29" />

I added WazuhLabUser to the local Administrators group using PowerShell to generate a controlled privileged group membership change.

### 7. Detecting the Administrators Group Change

<img width="1015" height="620" alt="image" src="https://github.com/user-attachments/assets/575eef00-9603-41ad-92f4-c8985f31759f" />

I verified that Wazuh reported an Administrators Group Changed alert under rule 60154, level 12.

### 8. Investigating Group Membership Details

<img width="900" height="933" alt="image" src="https://github.com/user-attachments/assets/5a65d3af-f7f4-4897-9822-ad9c61962f39" />

I examined Windows Security event ID 4732, which recorded a member being added to the local Administrators group.

### 9. Removing the Test User from Administrators

<img width="899" height="286" alt="image" src="https://github.com/user-attachments/assets/e8dffe51-3d63-4181-84ec-8a70e7d2fde0" />

I removed WazuhLabUser from the local Administrators group using PowerShell to reverse the privileged membership change.

### 10. Investigating the Group Removal Event

<img width="900" height="755" alt="image" src="https://github.com/user-attachments/assets/d55d81ef-e0f3-46c0-8583-c7e582897e2f" />

I reviewed Windows Security event ID 4733, which recorded a member being removed from the local Administrators group.

### 11. Deleting the Temporary Account

<img width="1022" height="297" alt="image" src="https://github.com/user-attachments/assets/ef4ebaab-4254-4697-a42a-dc0e84e7522c" />

I deleted WazuhLabUser after completing the tests. Windows confirmed that the command completed successfully.

### 12. Reviewing Account Deletion Alerts

<img width="1041" height="503" alt="image" src="https://github.com/user-attachments/assets/f4c38c0f-05d8-4b23-a714-bf047983cb51" />

I verified that Wazuh displayed alerts for account deletion and related group changes, documenting the cleanup activity.

### Part 3 Summary

I created a temporary Windows user, added and removed it from the local Administrators group, and deleted the account. I investigated the resulting Wazuh alerts, reviewing rule IDs, severity levels, Windows event IDs, and account information to understand how account and privilege changes were recorded.

## Part 4: File Integrity Monitoring Configuration and Investigation

### 1. Locating the Agent Configuration File

<img width="900" height="662" alt="image" src="https://github.com/user-attachments/assets/bdd0222b-41f9-463f-a0e2-df4266a0cfe4" />

I located ossec.conf in the Windows agent installation folder to configure File Integrity Monitoring.

### 2. Configuring Real-Time Folder Monitoring

<img width="900" height="581" alt="image" src="https://github.com/user-attachments/assets/636c03d1-783e-4df1-9119-06d06bacfae1" />

I added C:\WazuhFIMLab to the syscheck configuration with realtime="yes" to monitor file changes in the test folder.

### 3. Restarting the Wazuh Agent

<img width="915" height="283" alt="image" src="https://github.com/user-attachments/assets/5e4fe906-9af5-4956-8920-9cf056b3fc36" />

I restarted the Wazuh agent service from an elevated PowerShell window to apply the configuration changes.

### 4. Creating the Test File

<img width="900" height="872" alt="image" src="https://github.com/user-attachments/assets/a486b99e-ca5c-47b6-a8f2-0668f87c3888" />

I created an empty text file named test.txt inside C:\WazuhFIMLab to begin the monitoring test.

### 5. Adding Content to the File

<img width="900" height="563" alt="image" src="https://github.com/user-attachments/assets/adfb52b2-4247-43ee-85c7-24d57f7abbb4" />

I entered “Wazuh FIM creation test” in Notepad and saved the file to generate a modification event.

### 6. Reviewing File Integrity Monitoring Events

<img width="990" height="580" alt="image" src="https://github.com/user-attachments/assets/7bc0f9c5-0585-4730-8a82-29888d1e14a2" />

I verified that Wazuh recorded added and modified events for test.txt. The event list also showed activity for the file’s original name before it was renamed.

### 7. Investigating the File Creation Alert

<img width="974" height="878" alt="image" src="https://github.com/user-attachments/assets/0be6702f-959e-4d61-8212-530a83c39114" />

I opened Document Details and confirmed that Wazuh detected test.txt being added in real time under rule 554, level 5.

### 8. Reviewing Additional Creation Alert Details

<img width="994" height="1027" alt="image" src="https://github.com/user-attachments/assets/e3ed49f2-8b77-4622-bc86-de45857fdb17" />

I captured another view of the file creation alert, showing the monitored file path, endpoint information, and Wazuh rule details.

### 9. Deleting the Test File

<img width="1005" height="491" alt="image" src="https://github.com/user-attachments/assets/3b28ff75-4ae6-4958-bdf9-3aa442c44fa5" />

I deleted test.txt after completing the creation and modification tests. File Explorer confirmed that the monitored folder was empty.

### 10. Verifying the File Deletion Alert

<img width="1005" height="638" alt="image" src="https://github.com/user-attachments/assets/87ccfee5-d2b5-4e56-a27f-a63363cd7f49" />

I confirmed that the FIM Events page recorded the deletion of test.txt under Wazuh rule 553, level 7.

### 11. Investigating File Deletion Details

<img width="1007" height="772" alt="image" src="https://github.com/user-attachments/assets/7642df2f-3ff7-475c-aaf5-b39c8ae004ab" />

I examined Document Details to verify the deleted file’s path, real-time detection mode, endpoint information, and associated rule.

### Part 4 Summary

I configured real-time monitoring for a test folder and generated file creation, modification, and deletion activity. I verified the resulting events in Wazuh and examined creation and deletion alert details to confirm the file paths, detection mode, and rule information.
