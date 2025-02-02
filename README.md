# SOAR-EDR: Automating Threat Detection & Response

## Table of Contents
1. [Introduction](#introduction)
2. [Project Setup](#project-setup)
4. [Conclusion](#conclusion)

## Introduction
This project provides hands-on experience with Security Orchestration, Automation, and Response (SOAR) and Endpoint Detection and Response (EDR) by integrating Lima Charlie and Tines. The goal is to create a detection and response workflow that can automatically identify threats, send alerts via Slack and email, and provide an option to isolate compromised machines.


## Workflow
### 1. Threat Detection
   - Lima Charlie detects suspicious activity.
   - Detection is forwarded to Tines for further processing.
### 2. Alert
   - Tines sends an alert to Slack and email, containing: <br>`Timestamp`<br>`Computer Name`<br>`Source IP`<br>`Process Command Line`<br>`File Path`<br>`Sensor ID`<br>`Detection Link (if application)`
### 3. User Decision & Response
   - The user will be prompted to either isolate the machine or not. If yes, LimaCharlie will automatically isolate the machine. If no, an alert will be sent to Slack.
### 4. Status Update
   - If isolation is successful/unsuccessful, Slack will receive the respective status update.
     


### Installation
- 


## Conclusion
Overall, honeypots serve as a great tool to analyze trends in malicious activity. Since T-Pot has the ELK Stack integrated into it, all of the data aggregated through various honeypots could easily be displayed using dashboards, which is what Kibana excels at. 

