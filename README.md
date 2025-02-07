# SOAR-EDR: Automating Threat Detection & Response

## Table of Contents
1. [Introduction](#introduction)
2. [Workflow](#workflow)
3. [Project Setup](#project-setup)
4. [Conclusion](#conclusion)

## Introduction
This project provides hands-on experience with Security Orchestration, Automation, and Response (SOAR) and Endpoint Detection and Response (EDR) by integrating Lima Charlie and Tines. [LimaCharlie](https://limacharlie.io/) is a Security Operations (SecOps) Cloud Platform that provides Endpoint Detection and Response (EDR), log collection, automation, and custom threat detection capabilities. [Tines](https://www.tines.com/) is a Security Orchestration, Automation, and Response (SOAR) platform that allows security teams to automate workflows without writing code. The goal is to create a detection and response workflow that can automatically identify threats, send alerts via Slack and email, and provide an option to isolate compromised machines.


## Workflow
### 1. Threat Detection
   - Lima Charlie detects suspicious activity.
   - Detection is forwarded to Tines for further processing.
### 2. Alert
   - Tines sends an alert to Slack and email, containing: <br>`Timestamp`<br>`Computer Name`<br>`Source IP`<br>`Process Command Line`<br>`File Path`<br>`Sensor ID`<br>`Detection Link (if application)`
### 3. User Decision & Response
   - The user will be prompted to either isolate the machine or not. If yes, LimaCharlie will automatically isolate the machine. If no, an alert will be sent to Slack.
### 4. Status Update
   - If isolation is successful/unsuccessful, Slack will receive the respective status update. Here's the workflow diagram: <br><p align="center"><img src="/images/SOAR Diagram.jpg" width=400 height=400></p>
     


## Project-Setup
- Windows Server:
   - For this lab, a Windows Server is necessary, as that's where our data will come from. I created a VM instance of the Windows Server 2025 version, though past versions should work just fine. Do note that you could also create a VM on the cloud using a cloud provider. 
- LimaCharlie:
   - Once you have configured the Windows Server, you'd need to create an account on LimaCharlie. After creating an account, head over to <b>Organization</b> and select a <b>Data Residency Region</b>.<br><p align="center"><img src="/images/limacharlie_org.png" width=500 height=400></p><br>
   - Don't forget to generate an <b>installation key</b> under <b>Sensors > Installation Keys</b>. Feel free to name it whatever.<br><p align="center"><img src="/images/create_installation_key.png" width=500 height=300></p><br>


## Conclusion


