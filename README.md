## Table of Contents

- [Introduction](#introduction)
- [Workflow](#workflow)
- [Project Setup](#project-setup)
- [Slack and Tines Integration](#slack-and-tines-integration)
- [Conclusion](#conclusion)

---

## Introduction

The **SOAR-EDR** project provides hands-on experience with integrating Security Orchestration, Automation, and Response (SOAR) with Endpoint Detection and Response (EDR) systems. This is achieved by combining **LimaCharlie** and **Tines**.

- ![LimaCharlie](https://limacharlie.io/) is a cloud-based platform offering Security Operations (SecOps) tools such as EDR, log collection, automation, and custom threat detection capabilities.
- ![Tines](https://www.tines.com/) is a SOAR platform designed to automate security workflows without the need for coding.

The goal of this project is to create an automated detection and response workflow that:
1. Detects threats
2. Sends alerts via Slack and email
3. Provides an option to isolate compromised machines


## Workflow
### 1. Threat Detection
   - Lima Charlie detects suspicious activity.
   - The detected event is forwarded to Tines for further processing.
### 2. Alert
   - Tines sends an alert to Slack and email, containing: <br>`Timestamp`<br>`Computer Name`<br>`Source IP`<br>`Process Command Line`<br>`File Path`<br>`Sensor ID`<br>`Detection Link (if application)`
### 3. Decision & Response
   - The user will be prompted to either isolate the machine or not.
        - If **yes**, LimaCharlie will automatically isolate the machine.
        - If **no**, an alert will be sent to Slack.
### 4. Status Update
   - If isolation is successful/unsuccessful, a status update will be sent to Slack. Here's the workflow diagram: <br><p align="center"><img src="/images/SOAR Diagram.jpg" width=600 height=600></p>
     


## Project-Setup
### Windows Server Setup

- For this project, you need a Windows Server instance, which can either be hosted on a local machine or a cloud provider. 
- A **Windows Server 2025** VM instance is recommended, but previous versions will also work.
  
### LimaCharlie Configuration
1. **Account Creation**: 
   - Sign up for a **LimaCharlie** account.
   - Once logged in, create an <b>Organization</b> and select a <b>Data Residency Region</b>.<br><p align="center"><img src="/images/limacharlie_org.png" width=500 height=400></p><br>
2. **Generate Installation Key**: 
   - Navigate to **Sensors > Installation Keys** to generate an installation key. You can name it as you see fit.<br><p align="center"><img src="/images/create_installation_key.png"></p><br>
3. **Sensor Installation**: 
   - Once you've logged into LimaCharlie, you should see something like this:<br><p align="center"><img src="images/limacharlie_default.png" width=500 height=300></p><br>
   - Before moving on, let's explore what LimaCharlie has to offer. On the right-hand side, you have options such as `Detections`, `Automation`, etc.<br><p align="center"><img src="images/limacharlie_home.png" width=200 height=700></p><br>
   - By now, you should have generated an <b>Installation Key</b>. When you scroll below on the <b>Installation Keys</b> page, you should see a list of sensor downloads for various operating systems. We're interested in an EDR sensor for Windows 64-bit, or to your respective version. <b>NOTE:</b>Make sure to copy the link of your respective EDR-sensor download and paste that into your Windows Server browser, which will download the actual agent.<br><p align="center"><img src="images/limacharlie_edr.png"></p><br>
4. **Run Installation Command**:
   - Once you've downloaded the agent onto your Windows Server, run the following command: <br>`cd Downloads`<br>`.\(file name).exe -i (YOUR_INSTALLATION_KEY)`<br>
   - Verify the agent is running in <b>LimaCharlie > Sensors List</b>. Here's my sensor (middle one):<br><p align="center"><img src="images/limacharlie_sensors.png"></p><br>
   - When you click on the sensor, you'll be brought to a page that lists information about the sensor, such as hostname, platform, etc. On the left, you'll see a list of features.<br><p align="center"><img src="images/limacharlie_features.png"></p><br>.
   - From this point, we'll start testing out certain features that LimaCharlie is known for, such as detecting suspicious activities based on given rules and performing automation to deal with threats.

### Testing LimaCharlie's Features
1. **Detecting Suspicious Activity**: 
   - Download ![LaZagne](https://github.com/AlessandroZ/LaZagne) (a tool used for password recovery) on your server. We'll be using this tool to test LimaCharlie. Use the following command to run it.<br>`.\lazagne.exe all`<br>
   - LimaCharlie will detect LaZagne's execution.
2. **Creating Detection & Response (D&R) Rule**:
   - Now we're going to create a <b>Detection & Response Rule</b>, which can be done by going to <b>LimaCharlie > Automation > D&R Rules</b>.<br><p align="center"><img src="images/original_rules.png"></p><br>
   - Copy and paste the text from ![D&R Rule](https://github.com/mujtabaa-adam/SOAR_EDR_Project/blob/main/D%26R%20Rule) on to the respective sections.<br><p align="center"><img src="images/limacharlie_dr.png"></p><br>
3. **Testing the Rule**:
   - To test the rule, simply scroll down and click `Target Event`. Here, we'll simply paste an event, which we will now find.<br><p align="center"><img src="images/limacharlie_testev.png"></p><br>
   - To find the event, make sure you've run LaZagne, as that'll generate an event in LimaCharlie. To find it, head to <b>Sensor > Timelime</b>. It'll look like this:<br><p align="center"><img src="images/limacharlie_timeline.png"></p><br>
   - Searching for LaZagne will give us this:<br><p align="center"><img src="images/limacharlie_timeline2.png"></p><br>
   - Click on any of the events that mention <b>NEW_PROCESS</b> and copy that event. Head back to `Target Event` to test our rule. Once you paste it in and click `Test Event`, it'll show this:<br><p align="center"><img src="images/rule_match.png"></p><br>
   - This indicates that our rule works without any errors. Make sure to save the rule. At this point, LimaCharlie will be able to detect any events that trigger our rule and it'll display that in the `Detections` tab. Here's what it'll look like if you run LaZagne again:<br><p align="center"><img src="images/limacharlie_detections.png"></p><br>
6. **View Detections**: 
   - After triggering the event, you can view detection details, such as:
     - Hash
     - Process ID
     - Additional metadata that will assist in investigating malicious activity.
7. **Automated Actions**: 
   - Based on the detected events, you can configure LimaCharlie to perform automated actions, such as isolating the machine.

### Slack and Tines Integration

Follow these steps to integrate Slack and Tines with LimaCharlie for automated threat detection alerts:

1. **Create Accounts & Slack Channel**
   - Sign up for **Slack** and **Tines** if you haven’t already.
   - In Slack, create a channel named `#alerts` to receive security notifications.

2. **Configure Tines**
   - Create a new **Story** in Tines.
   - Delete all default elements.
   - Add a **Webhook** and name it appropriately.
   - Copy the **Webhook URL** generated.

3. **Integrate LimaCharlie with Tines**
   - In LimaCharlie, navigate to `Outputs > Add Output > Detections`.
   - Select **Tines** as the output type.
   - Enter a name and paste the **Webhook URL** from the previous step.
   - Save the output configuration.<br><p align="center"><img src="images/link_detections.png"></p><br><p align="center"><img src="images/link_tines.png"></p><br>
   
4. **Testing the Integration**
   - Run **LaZagne** again to generate a detection.
   - Go to `Outputs` in LimaCharlie; you should see an entry similar to the one below:<br><p align="center"><img src="images/limacharlie_outputs.png"></p><br>

5. **Verify in Tines**
   - Click **"View Samples"** in LimaCharlie to inspect the detection data.
   - In Tines, go to your **Webhook > Events** to confirm that the detection has been received.
   - If the event appears in Tines, the integration is successfully configured.

### Implementation
- Currently, our Tines playbook is empty, other than just having the detections webhook. We're going to add Slack to send alerts to the #alerts channel. To do this, simply go to the Templates > Search for Slack > Drag & Drop > Search for "Send a Message" > Paste the following for the Message field: Title - <<detections.body.cat>>
``Event Time (Epoch) - <<detections.body.detect.routing.event_time>>
Hostname - <<detections.body.detect.routing.hostname>>
IP - <<detections.body.detect.routing.int_ip>>
Username - <<detections.body.detect.event.USER_NAME>>
File Path - <<detections.body.detect.event.FILE_PATH>>
Command - <<detections.body.detect.event.COMMAND_LINE>>
Sensor ID - <<detections.body.detect.routing.sid>>
Detection Link - <<detections.body.link>>``

- Make sure to copy and paste the #alerts channel ID. Here's what it should look like, as well as the playbook:<br><p align="center"><img src="images/slack_1.png"></p><p align="center"><img src="images/playbook_1.png"></p><br>
- Now, we need to do the same, but for our email. Drag and drop the Send Email button and paste the following as the message: ``<b>Title</b> - <<detections.body.cat>>
<br><b>Event Time (Epoch)</b> - <<detections.body.detect.routing.event_time>>
<br><b>Hostname</b> - <<detections.body.detect.routing.hostname>>
<br><b>IP</b> - <<detections.body.detect.routing.int_ip>>
<br><b>Username</b> - <<detections.body.detect.event.USER_NAME>>
<br><b>File Path</b> - <<detections.body.detect.event.FILE_PATH>>
<br><b>Command</b> - <<detections.body.detect.event.COMMAND_LINE>>
<br><b>Sensor ID</b> - <<detections.body.detect.routing.sid>>
<br><b>Detection Link</b> - <<detections.body.link>>``

- Make sure to also put the email address on the respective fields and put "Alerts" as the Sender name, and "Alert" for the Subject.

- Now we need to add a page for our user prompt. To do this, head to Tools > Page > Drag & Drop. Change the name to User Prompt. Double click it and format it like this:<br><p align="center"><img src="images/user_prompt.png"></p><br>
- Here's how the playbook should look:<br><p align="center"><img src="images/playbook_2.png"></p><br>
- NOTE: Before proceeding, make sure to test the playbook, as you'll need information from the events in order to automate a response for them. Drag and drop two Trigger buttons. Name them as "Yes" and "No," or any other names you deem to be fit. Type from the following image onto one of the Triggers

## Conclusion


