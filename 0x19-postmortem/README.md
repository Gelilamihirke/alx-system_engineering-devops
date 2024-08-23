![img1 (2)](https://github.com/user-attachments/assets/8179724f-4b67-4ebe-b395-ef79c8670879)

Issue Summary
Duration of the Outage:
The outage started at 2:15 PM and was resolved by 3:30 PM GMT on August 18, 2024.

Impact:
The entire website was down during the outage, with 100% of users unable to access the platform. Users encountered a "503 Service Unavailable" error when trying to reach the site. This caused a complete halt in service, impacting all web traffic and preventing any user activity.

Root Cause:
The root cause was a misconfiguration in the Nginx server. Specifically, the site configuration in the sites-available directory was not linked to sites-enabled, which meant that Nginx wasn't properly set to listen on port 80.

Timeline
2:15 PM:
The outage was detected by our monitoring system, which triggered an alert due to multiple failed health checks. Simultaneously, customer complaints began coming in about site inaccessibility.

2:20 PM:
The issue was escalated to the technical operations team for immediate investigation. Initial attempts to access the site confirmed the issue, with all requests failing with a "503 Service Unavailable" error.

2:30 PM:
The technical team began investigating the Nginx configuration, reviewing the logs and checking for any syntax errors in the configuration files.

2:45 PM:
After confirming that there were no syntax errors, the investigation shifted to checking the Nginx service's active ports. It was discovered that Nginx was not listening on port 80, indicating a possible configuration issue.

2:55 PM:
Further inspection revealed that the configuration file in the sites-available directory was not linked to sites-enabled. This oversight prevented the configuration from being loaded by Nginx.

3:10 PM:
The missing symbolic link was created, and the Nginx service was restarted to apply the changes.

3:20 PM:
Post-resolution verification checks were conducted to ensure that the site was fully operational and Nginx was serving traffic correctly on port 80.

3:30 PM:
The incident was officially resolved, with the site fully restored and operational.

Root Cause and Resolution
Root Cause:
The root cause was an oversight in the Nginx configuration setup. The configuration file in the sites-available directory was not linked to sites-enabled, which meant that although the configuration was correct, it was never activated. As a result, Nginx was not listening on port 80, causing the site to be inaccessible.

Resolution:
To resolve the issue, the following steps were taken:

A symbolic link was created from /etc/nginx/sites-available/default to /etc/nginx/sites-enabled/default.
The Nginx service was restarted to apply the updated configuration.
Verification checks were performed to ensure the site was accessible and functioning as expected.
Corrective and Preventative Measures
Improvements:

Automation: Implement an automated process to ensure that configuration files in sites-available are always linked to sites-enabled after any updates.
Monitoring: Enhance the monitoring system to include checks for Nginx’s active listening ports, ensuring such issues are detected before they cause an outage.
Configuration Review: Implement a mandatory post-deployment review process to confirm that all necessary configurations are active.
Task List:

Automate Configuration Linking: Create a script that automatically links configuration files from sites-available to sites-enabled after any changes.
Enhance Monitoring: Add monitoring checks to verify that Nginx is actively listening on port 80 and that the correct site configuration is enabled.
Configuration Review Process: Update the deployment checklist to include verification that the necessary Nginx configurations are linked and active before completing the deployment.
Here’s an example of the automation script:
#!/usr/bin/env bash
# Script to ensure Nginx configuration is properly linked and Nginx is restarted

ln -sf /etc/nginx/sites-available/default /etc/nginx/sites-enabled/default
sudo service nginx restart
By implementing these corrective measures, we aim to prevent similar incidents in the future and improve the overall reliability of our web services.
![NGINX](https://github.com/user-attachments/assets/371f7246-eb57-493c-8992-8f406f7e6fe0)
