🔹 Automating alert management with Azure Logic Apps 🔹
I have just completed development of my first Logic App on Azure, a workflow that automates the management of alerts received via email and integration with Azure Sentinel for creating security incidents.

🔍 How does it work?
✅ Trigger: The Logic App is triggered when a new email arrives in a specific box (Office 365).
✅ Condition Check: Checks whether the email subject line ends with the word “done.” If yes, the flow stops.
✅ Dynamic waiting: If the email does not contain “done,” the Logic App waits 9 minutes and retrieves the last email to check if a confirmation has arrived.
✅ Data parsing and normalization: If there is no confirmation email, the app extracts information from the HTML of the message, removes tags, and converts the content to text.
✅ Incident creation on Azure Sentinel: If conditions are met, the Logic App automatically generates an incident on Sentinel with alert details, including host, signature, impact, and managed object.

🛠 Technologies used:
🔹 Azure Logic Apps for workflow orchestration.
🔹 Office 365 Connector for email tracking.
🔹 Azure Sentinel Connector for incident management
🔹 Conversion Service for parsing email body from HTML to text

📌 Benefits of this solution:
✔️ Automation of alert triage without manual intervention
✔️ Seamless integration between M365 and Azure Sentinel for security management
✔️ Improved incident response time through immediate ticket creation on Sentinel
