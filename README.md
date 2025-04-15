
![LogicApp](https://github.com/user-attachments/assets/12f00dfe-1c21-4a46-a832-870ba8ae34ce)

Logic App Analysis - Step by Step
This Logic App is designed to monitor the Office 365 inbox, search for specific emails, and automatically create an incident on Microsoft Sentinel based on the information extracted from the email.

1️⃣ Trigger - When a new email arrives.
The flow starts with a trigger that is triggered when a new email arrives in the configured inbox.

Trigger: “When_a_new_email_arrives_(V3)”

Connection: Office 365

Filter: Checks all emails in Inbox, no attachments required.

Method: GET on /v3/Mail/OnNewEmail.

📌 Effect: Whenever a new email arrives, the stream is triggered.

2️⃣ Check if the subject of the email contains “done”
The Logic App checks if the email subject ends with “done”.

If the subject ends with “done”, then an incident is NOT created.

If “done” is not present, the Logic App waits 9 minutes and goes to the next step.

📌 Effect: This filter prevents already handled emails (“done”) from triggering the flow again.

3️⃣ After 9 minutes, it checks if an email with “done” exists.
After a 9-minute wait, the Logic App makes another request to Office 365 to check if an email with the same subject but with “done” added has arrived.

Action: “Get_emails_(V3)”


Method: GET on /v3/Mail

Query: Search email with original subject + “ done”.

📌 Effect: If it finds a confirmation email, it stops the flow. Otherwise, it continues with the creation of an incident.

4️⃣ Checks for matching emails.
If it finds one or more matching emails, it starts a For Each (“For_each”) loop to analyze all the emails found.

Check the content of the emails to see if they were received more than 8 minutes after the original email.

📌 Effect: Only emails received more than 8 minutes after the original are considered valid.

5️⃣ If no “done” email is found, it creates an incident on Sentinel.
If no confirmation email is found within 9 minutes, the Logic App initiates the creation of an incident on Microsoft Sentinel.

Step 1: Converts the email to pure text (“Convert_Html_To_Text”).

Step 2: Extracts key information from the email:

Alert ID → Extracted from the subject (number after #).

Host → Extracted from the email body.

Signatures → Type of suspicious traffic.

Impact → Data on the severity of the attack.

Managed Object → Name of the managed object.

📌 Effect: Custom tags are generated for the new incident on Sentinel.

6️⃣ Creating the incident on Microsoft Sentinel.
If the check fails and no “done” email is found, an incident is created in Azure Sentinel.

Method: PUT on /Incidents/subscriptions/.../resourceGroups/.../workspaces/...

Title: “New Arbor Alert received by email - #<AlertID>”

Description: Email content in text format.

Tags:

Host

Signatures

Impact

Managed Object

📌 Effect: The incident is automatically assigned to the SOC, with detailed information about the attack.

📌 Flow Summary.
1️⃣ Trigger: When a new email arrives.
2️⃣ Check: If the subject ends with “done,” the flow stops.
3️⃣ Wait 9 minutes to see if a confirmation email arrives.
4️⃣ Check matching emails (for more than 8 minutes).
5️⃣ If no confirmation exists, converts email to text and extracts information.
6️⃣ Create an incident on Microsoft Sentinel with extracted details.
