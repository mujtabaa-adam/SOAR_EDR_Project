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
- Post-LimaCharlie:
   - Once you've logged into LimaCharlie, you should see something like this:<br><p align="center"><img src="images/limacharlie_default.png" width=500 height=300></p><br>
   - Before moving on, let's explore what LimaCharlie has to offer. On the right-hand side, you have options such as `Detections`, `Automation`, etc.<br><p align="center"><img src="images/limacharlie_home.png" width=200 height=700></p><br>
   - By now, you should have generated an <b>Installation Key</b>. When you scroll below on the <b>Installation Keys</b> page, you should see a list of sensor downloads for various operating systems. We're interested in an EDR sensor for Windows 64-bit, or to your respective version. <b>NOTE:</b>Make sure to copy the link of your respective EDR-sensor download and paste that in your Windows Server browser, which will download the actual agent.<br><p align="center"><img src="images/limacharlie_edr.png"></p><br>
   - Once you've downloaded the agent onto your Windows Server, run the following command: <br>`cd Downloads`<br>`.\(file name).exe -i (YOUR_INSTALLATION_KEY)`<br>
   - Verify the agent is running in <b>LimaCharlie > Sensors List</b>.
                                                                                                                                                                         


## Conclusion


