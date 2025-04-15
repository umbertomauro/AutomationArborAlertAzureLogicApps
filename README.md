# AutomationArborAlertAzureLogicApps
Logic app on azure to automate Arbor alert emails that arrive on an outlook mailbox

This Azure Logic App is specifically designed to work with Arbor DDoS alerts and generate incidents in Microsoft Sentinel when certain conditions are met.

🔄 Workflow Summary:
Trigger – When a new email arrives (V3)
The Logic App is triggered whenever a new email is received—specifically, an alert email from Arbor regarding a potential DDoS attack.

Check: Is this a clone email?
It checks whether a similar Arbor alert has already been received and processed:

If yes (True) → The process stops (to avoid duplicate incidents).

If no (False) → It continues with further checks.

Delay + Get Emails:

Introduces a short delay (to ensure all related emails are available).

Retrieves recent emails to look for similar alerts from Arbor.

Condition: Does a clone email exist?
It checks if other similar alerts (potential duplicates) already exist.

Loop + Match Check (if clones exist):

For each similar email, it verifies whether it is a valid match (based on criteria such as IPs, attack vector, or other metadata).

If a valid match is found:

The email is parsed (HTML → text).

An incident is created in Microsoft Sentinel.

If no clones are found:

The current email is treated as a new alert.

Parsed and converted to text.

An incident is created in Sentinel.

⏱️ Key Condition – 8-Minute Threshold:
This Logic App is built to trigger only when a DDoS attack reported by Arbor lasts longer than 8 minutes.
This 8-minute threshold is configured on Arbor's side, so the email is only sent if the condition is met. When that email arrives, the Logic App creates an incident in Sentinel accordingly.
