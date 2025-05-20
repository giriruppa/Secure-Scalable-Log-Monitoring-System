### 🚀 Project Scope
The objective of this project is to design, develop, and deploy a secure and scalable log monitoring system using Docker containers and Splunk Enterprise. The system automates ingestion, analysis, and alerting for security-centric logs from distributed systems—enhancing visibility, incident detection, and operational efficiency.

### 🔍 Key Focus Areas:
- Centralized Log Monitoring
Aggregation of logs from multiple sources (web servers, applications, authentication services) into a single Splunk instance.

- Docker-Based Deployment
Use of Docker to containerize Splunk Enterprise and Splunk Universal Forwarders, ensuring environment consistency, scalability, and portability.

- Security and Real-Time Alerting
Configuration of alerts for suspicious activity like failed logins, incorrect password attempts, and unauthorized access.

- Scalability
Horizontal scalability by deploying additional forwarders or indexers as needed.

- CI/CD Integration
Automated deployment and updates through Jenkins pipelines for improved reliability and continuous delivery.

- Optional Cloud Integration
Extension to AWS for log storage (S3), identity/access control (IAM), and long-term log analysis.

🛡️ This system aims to be an enterprise-grade, real-time log visibility and threat detection solution across dynamic infrastructures.

### 🧱 System Architecture
The architecture ensures secure, scalable, and automated log monitoring using Docker, Splunk, and CI/CD integration. The components are modular and can be expanded horizontally for performance and reliability.

### 📷 Architecture Diagram
![image](https://github.com/user-attachments/assets/6fb68c4a-34e7-431b-800c-a71433bd31d8)

| Component                  | Description                                                      |
| -------------------------- | ---------------------------------------------------------------- |
| Splunk Enterprise          | Central log management and analysis platform                     |
| Splunk Universal Forwarder | Lightweight log forwarder installed on source systems            |
| Docker                     | Container platform to ensure consistent and portable deployment  |
| Jenkins                    | Automates CI/CD for deployment and updates                       |
| AWS (Optional)             | Cloud storage (S3), IAM for security, and long-term log archival |




### Implementation Details
1. Tools and Technologies Used
To create a secure and scalable log-monitoring platform, the following tools and technologies were used:
- Docker: For the containerization of the Splunk components and services themselves.
-	Splunk Enterprise: For log ingestion, searching, and alerting.
-	Splunk Universal Forwarder: Lightweight agent used to forward logs from the endpoint systems.
-	Jenkins: For CI/CD automation.
-	GitHub: Version control and repository for Jenkins integration.
-	Linux (Ubuntu/Kali) and Windows: OS environments for Splunk and Forwarders applications.
-	Docker Compose: For orchestration of multi-containers.
________________________________________
2. Docker Configuration
With Splunk components containerized in Docker, this made them able to be deployed in isolation and therefore in reproducible manner:
-	Splunk Enterprise Container:
            -    Docker image pulled from Splunk Docker Hub repo
            -    Set up with ports 8000, 8089, and mounted volumes for persistence.
            -    Credentials and license acceptance were set through environment variables
-	Docker Compose File:
-    Manage service lifecycle (up/down).
-    Used to bring up splunk-enterprise service by a single command.
 ![image](https://github.com/user-attachments/assets/5aa551ca-ac56-49de-afdb-0ea2eeece1c7)

________________________________________
3. Splunk Setup
-	Open the Splunk Enterprise Web Interface, by hitting http://<host-ip>:8000.
-	Configured indexes and data inputs for log collection.
-	Installed and configured Splunk Universal Forwarder on a remote VM:
-	Used /opt/splunkforwarder/bin/splunk add forward-server <IP>:9997.
	Added file monitoring inputs:
   ./splunk add monitor /var/log/auth.log.
 	![image](https://github.com/user-attachments/assets/7dd362ab-3a04-4472-8b89-8cf7ec5ba933)


________________________________________
4. Jenkins CI/CD Integration
-	Jenkins Pipeline created with a Jenkinsfile hosted in GitHub.
-	Stages contained:
-	GitHub Checkout 
-	Repository Cloning
-	Splunk Deployment using Docker-Compose
-	Jenkins monitored the Git repo for changes and redeployed automatically on push.
stage:
 ![image](https://github.com/user-attachments/assets/00c738ac-401a-4de8-a5a0-060f5b2b7558)

 ________________________________________



### Data Flow & Log Collection
Confidently maintaining a superior and pragmatic log-monitoring system is dependent on the seamless integration of data flow from multiple sources to a central repository for indexation, searching, and analysis. The project implemented a secure and scalable pipeline for log data into the Splunk Universal Forwarder and Splunk Enterprise to ensure high availability and accuracy of logs for both security monitoring and operational analysis. 

________________________________________
1. Log Sources
Log sources are the beginning of event and activity data generation. This includes systems, applications, servers, or network devices. From our side, we have chosen quite a few types of log-generating sources such as: 
-	Authentication logs for Linux systems (e.g., /var/log/auth.log) 
-	System logs from /var/log/syslog or /var/log/messages 
-	Web application logs (e.g., Apache, NGINX access logs) 
-	Logs that capture security incidents such as failed login attempts or unauthorized access 
-	Logs from Docker containers where each service container generates stdout/stderr logs 
The variety of sources ensures that the monitoring system can offer great visibility into the behaviour and security events taking place within the system.
________________________________________
2. Splunk Universal Forwarder
The Splunk Universal Forwarder (UF) is a lightweight data-collecting agent that is installed mainly on edge machines (in this case, a Kali Linux VM within VirtualBox). The main duty of the Universal Forwarder is to securely collect raw logs from local sources and then forward them centrally to be indexed and analyzed from that Splunk Enterprise instance.
Such Key Features and configurations:
-	Installed as a Docker container inside the Kali VM
-	inputs.conf file editing to specify the paths of the monitored logs 
-	Strictly secured over port 9997 (which is the default Splunk TCP port)
-	Deployment server model which manages multiple forwarders (optional)
-	Real-time log forwarding with minimum resource utilization
Thus, this important component ensures the sensitive logs at various remote endpoints remain captured and forwarded efficiently.

________________________________________
3. Indexing and Searching (Splunk Enterprise)
Splunk Enterprise on the receiving end (installed on a Docker container) constitutes the centralized indexing and search engine.
Process breakdown:
-	Receiving logs via TCP input from the Universal Forwarder (either set in inputs.conf or in the Splunk UI)

-	Log parsing into specific indexes (e.g. auth_index, web_index) and log indexing
-	Structuring data into events with timestamps, source types, and host associated
-	Search Processing Language (SPL) is used by the end-users for querying and analyzing the logs
-	Dashboards and visualization provide insights into security trends, failed logins, and anomalies
Example SPL search:
index=auth_index sourcetype=linux_secure "Failed password"
Retrieving failed SSH login attempts to trigger alerts.



### Alerting Mechanism
This was among the principal tasks undertaken by this project: establishing real-time monitoring and proactive alerting on system log entries where users have performed suspicious or noteworthy activities. Alerts are among essential characteristics of every log monitoring system that will notify security teams about anomalies—especially events with the potential of unauthorized access attempts, policy violations, or system errors. In this case, we have created email alerts using Splunk Enterprise as the technology platform for real-time threat detection and notification purposes.



### Setting up Email Alerts in Splunk Enterprise
Splunk Enterprise offers a built-in method to configure alerts and notifications through many different actions such as sending an email or executing a script in conjunction with other third-party services like Slack, PagerDuty, and ServiceNow.

Here are the steps for configuring email alerts in Splunk Enterprise:
- Step 1: Enable Email Settings in Splunk
1. Go to Settings → Server settings → Email settings in Splunk.
2. Fill in the SMTP details of your email service provider. For example:
a. Mail Host: smtp.gmail.com:587
b. Authentication: Yes
c. Username: girirupppa964@gmail.com
d. Password: App Password (for Gmail with 2FA)
e. TLS/SSL: Enable STARTTLS
3. Save the configuration and test the email receipt.
- Step 2: Write a Search Query
Build a SPL query for Splunk that specifies what condition should be included for the alert. To, for example, detect failed log-on attempts, the following would be a frequently used query: 
index=forwarded_logs sourcetype=auth_logs "authentication failure" OR "failed password"
This query checks logs forwarded by the Splunk Universal Forwarder for keywords that indicate failed authentication attempts.
- Step 3: Creating and Scheduling the Alert
1. Visit Search & Reporting. 
2. Run your SPL query to check that it gives relevant results. 
3. Click Save As → Alert. 
4. Add relevant details: 
a. Alert Title: Detected a Failed Password Attempt 
b. Alert Type: Scheduled or Real-Time
c. Condition to Trigger: If results are greater than 0 
d. Time Range: Last 5 minutes (can be adjusted)
5. Under Trigger Actions, choose Send email. 
a. Add email recipient addresses. 
b. Customize subject line and message body (result tokens if needed). 
6. Save alert.
________________________________________





### Sample Use Case: Wrong Password Detection
Now, let's review an implementation for a situation in which an erroneous password in a Linux system is entered multiple times, triggering an email notification for such an event.
Scenario:
A malevolent intruder or just an incompetent user is unknowingly attempting to gain entrance into a protected website or SSH terminal with an invalid password. We want to make sure this action is logged by the Splunk Universal Forwarder in Splunk Enterprise's records.
Step-by-Step Flow:
1.	     Log Generation:
- A log of the authentication failure should be generated on the monitored machine (Kali Linux) in the /var/log/auth.log file.
- Failed password for invalid user admin from 192.168.1.100 port 2222 ssh2
2.	Log Forwarding:
- Configure the Splunk Universal Forwarder installed on the machine to monitor /var/log/auth.log and forward events to the Splunk Enterprise instance in Azure.
3.	Log Indexing and Searching:
- This log is then indexed by Splunk Enterprise, which will use a custom index, such as index=forwarded_logs.
4.	Detection Query in Splunk:
- The SPL query scans for words like "Failed password" and "authentication failure", or similar patterns in the logs:
- 	index=forwarded_logs sourcetype=linux_secure "Failed password"
5.	Triggering the Alert:
- 	An event-based alert is incorporated into the workings.
-	Set the alert to poll ever five minutes and mail if at least one matching event is found.
6.	Email Notification:
-	Takes place when an email is sent to the system administrator, with a subject as follows:
-	Alert: Failed Login Detected from 192.168.1.100
-	The body of the email contains the actual log entry, along with the timestamp and IP address of the source.
- Results:
The alert enables the security or operations team to react in real-time to initiate prompt corrective action-whether blocking an IP, disabling an account, or sweeping in for the forensic audit.





### Log Sample Output Screenshots
Here are some sample screenshots with a short description of the output collected during the testing phase:
________________________________________
1. Splunk Dashboard View:
-	Description: A screenshot to show the Splunk dashboard with the forwarded logs during capture.
-	Details Captured: Log source, timestamp, severity level, host machine, and message contents.  
Example: 
The log entry from /var/log/auth.log on Kali Linux showing the failed login attempt due to an incorrect password.
________________________________________
2. Search Query (SPL) Result:
-	Description: SPL query result for unsuccessful authentication attempts.
-	Query Used:
-	index=main sourcetype=linux_secure "Failed password"
-	Result: Provides a list of failed login attempts with the accompanying timestamp and IP address information.
________________________________________
3. Triggered Alert Screenshot (Email):
-	Description: An email alert screenshot displaying the state triggered (e.g., "Failed password" log event).
-	Subject: "Alert: Unauthorized Login Attempt Detected".
-	Body: Comprises time with source IP and log context.
________________________________________
4. Docker Container Logs:
-	Description: Output from docker ps and docker logs commands.
-	Reason: In order to verify health and running Splunk services within the container. 

________________________________________
5. Jenkins Console Output:
-	Description: Output of a successful pipeline run on Jenkins. 
-	Stages Shown: Checkout from GitHub, Docker Compose Build and Up, deployment confirmation.

### Appendix

- A. Jenkinsfile

![image](https://github.com/user-attachments/assets/bfe0b2e3-fe40-47fa-8639-5ae17a045ab1)
![image](https://github.com/user-attachments/assets/9ab229b2-c10a-4477-9ac9-83d13faabdb2)

- B. Docker Compose File – splunk-enterprise/docker-compose.yml
![image](https://github.com/user-attachments/assets/6826367e-98cf-48f7-919f-1a8db35e4747)

- C. Docker Compose File – universal-forwarder/docker-compose.yml
![image](https://github.com/user-attachments/assets/623e9e76-d739-4349-b18d-b7e327513d91)

- D. Splunk Configuration Snippet – inputs.conf (For Forwarder)
- [monitor:///var/log/auth.log]
- disabled = false
- index = main
- sourcetype = linux_secure

### Build Output 

![image](https://github.com/user-attachments/assets/15a91a94-70c2-4a9e-9446-56d749f46343)
![image](https://github.com/user-attachments/assets/f4c971b8-b2d9-4688-bf24-d3e7bf1ed032)

![image](https://github.com/user-attachments/assets/e27adbce-2cef-4860-ad64-4ab20ec3048f)
![image](https://github.com/user-attachments/assets/44770da0-4f05-42b8-873c-e4ba822c6de3)
