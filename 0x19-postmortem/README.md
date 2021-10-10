# Postmortem: config file failure caused a website downtime

## Executive Summary

Duration of problem: Mon, October 7, 17:10 to 18:30 EAT,

##### impact: 
- the server was responding with 500 error code and any of the subdomains and the main domain was not responding accordingly for nearly 2 hours. 3000 requests from users were responded with 5xx error code.

##### Root Cause:
- the NGINX configuration was edited by one of the engineers and restarted the server with out checking for syntax.
##### Timeline (EAT
- 16:00- users started reporting the error
- 16:30 — the team noted the frequent reports and started assessing the situation
- 16:40- every hardware and server was checked for a hardware failure
- 17:00- the error on the config file of NGINX was detected, and it was corrected
- 17:00- the NGINX restart
- 17:30- The site got back to Normal

##### Resolution and Recovery
- At 16:30 AM, a ticket was opened to fix the down server and sent out to the database service provider personnel. The team arrived at 3:15 AM.
- The NSX CLI is used to get detailed information about tail logs and take packet captures. It also, looks at the metrics for trouble shooting the load balancer.
- Verification of basic services are running by checking the routing table.
- To help with recovery, we retarted affected servers. SRE engineers, manually restarted unicorn processes on the web application servers to further correct the reboot process.
- The process was slow to prevent any possible cascading failures and to avoid a wide scale reboot — affecting our customers even further.
- By 17:30 AM, traffic was restored and 100% of our customers reported back with no issues.
##### Corrective and Preventative Measures

> In the last 3 days, we’ve made an internal review and analysis of the outage. The following measures are actions the team will follow for prevention and to improve response times in the future.

- Monitoring mechanisms like data-dog must be used.
- The database server provider during the performance degradation could have alerted teams by noting that the down server was not routine behavior. An update to their systems to prevent further issues in the future are done accordingly.
- The Security Team did not properly plan the reboot schedule affecting many of our customers. There was no available staff to monitor the reboot and to access the situation right away in a timely manner.
- There were no engineers present to possibly resolve the situation by manually fixing any issues that came up.
- For scheduled reboots like this, there should be a 24 hour operations rotation in order to monitor every aspect of the update.


## Author
- Amanuel Sisay
