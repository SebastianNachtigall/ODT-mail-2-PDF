# ODT-mail-2-PDF
A little Google Script that scans your inbox for a specific mail (with a label) and a ODT attachment. It then converts the ODT file to a PDF and sends it out as another email.



Automating ODT to PDF Conversion and Email Forwarding with Google Apps Script

This guide provides step-by-step instructions to automate the process of converting ODT (OpenDocument Text) files received via email into PDFs and forwarding them to a mailing list using Google Apps Script. The automation includes setting up a Gmail filter, creating a Google Apps Script, enabling necessary APIs, and setting up triggers for automatic execution.

Table of Contents

	•	Prerequisites
	•	Overview
	•	Step 1: Set Up a Gmail Filter
	•	Step 2: Create and Set Up the Google Apps Script
	•	2.1: Create a New Google Apps Script Project
	•	2.2: Enable the Advanced Drive Service
	•	2.3: Enable the Drive API in Google Cloud Platform
	•	2.4: Add the Script Code
	•	2.5: Customize the Script
	•	Step 3: Authorize and Test the Script
	•	Step 4: Set Up Triggers for Automation
	•	Additional Considerations
	•	Error Handling
	•	Quotas and Limits
	•	Security Best Practices
	•	Conclusion

Prerequisites

	•	A Google account with access to Gmail and Google Drive.
	•	Basic familiarity with Google Apps Script (helpful but not required).

Overview

The automation process involves:

	1.	Gmail Filter: Labels incoming emails with ODT attachments.
	2.	Google Apps Script: Monitors labeled emails, converts ODT attachments to PDFs, and forwards them.
	3.	Drive API: Enables file conversion from ODT to PDF.
	4.	Triggers: Automates the script execution at regular intervals.

Step 1: Set Up a Gmail Filter

First, create a Gmail filter to label incoming emails with ODT attachments.

Steps:

	1.	Open Gmail:
	•	Log in to your Gmail account.
	2.	Create a New Filter:
	•	Click on the Search options icon (🔍) in the Gmail search bar.
	•	In the search window:
	•	Has attachment: Check this box.
	•	Includes the words: Type filename:odt.
	•	Click on “Create filter”.
	3.	Apply a Label:
	•	In the filter options:
	•	Check “Apply the label”.
	•	Click “Choose label” and select “New label…”.
	•	Enter ODT_Processing as the label name.
	•	Click “Create”.
	•	Click “Create filter” to save.
	4.	Verify the Filter:
	•	Send a test email with an ODT attachment to yourself.
	•	Check that the email is labeled with ODT_Processing.

Step 2: Create and Set Up the Google Apps Script

2.1: Create a New Google Apps Script Project

	1.	Access Google Apps Script:
	•	Go to script.google.com.
	2.	Create a New Project:
	•	Click on “New project”.
	3.	Name Your Project:
	•	Click on “Untitled project” at the top and rename it (e.g., “ODT to PDF Converter”).

2.2: Enable the Advanced Drive Service

	1.	In the Apps Script Editor:
	•	Click on the ”+” icon next to “Services” on the left sidebar.
	•	(If using the legacy editor, go to “Resources” > “Advanced Google services…”).
	2.	Enable Drive API:
	•	Scroll down to “Drive API”.
	•	Click the toggle switch to turn it ON.
	•	Click “OK”.

2.3: Enable the Drive API in Google Cloud Platform

	1.	Open the Cloud Platform Project:
	•	In the Apps Script editor, click on “Project Settings” (gear icon).
	•	Under “Google Cloud Platform (GCP) Project”, click on the link to the GCP project.
	2.	Enable the Drive API:
	•	In the GCP console, ensure you’re in the correct project.
	•	Navigate to “APIs & Services” > “Library”.
	•	Search for “Drive API”.
	•	Click on “Google Drive API”.
	•	Click the “Enable” button.
	3.	Ignore Credentials Banner:
	•	If prompted to create credentials, you can ignore this. Apps Script handles authentication internally.

2.4: Add the Script Code

Copy and paste the following code into the script editor, replacing any existing code:
```
function processODTEmails() {
  var label = GmailApp.getUserLabelByName("ODT_Processing");
  var threads = label.getThreads();

  for (var i = 0; i < threads.length; i++) {
    var messages = threads[i].getMessages();

    for (var j = 0; j < messages.length; j++) {
      var attachments = messages[j].getAttachments();

      for (var k = 0; k < attachments.length; k++) {
        var attachment = attachments[k];

        if (attachment.getContentType() === "application/vnd.oasis.opendocument.text") {
          try {
            // Save ODT to Drive
            var file = DriveApp.createFile(attachment);

            // Convert ODT to Google Doc using Advanced Drive Service
            var fileBlob = attachment.copyBlob();
            var convertedFile = Drive.Files.insert(
              {
                title: file.getName(),
                mimeType: 'application/vnd.google-apps.document'
              },
              fileBlob,
              {
                convert: true
              }
            );

            // Export Google Doc as PDF
            var pdfBlob = DriveApp.getFileById(convertedFile.id).getAs('application/pdf');

            // Get the current date and format it
            var currentDate = Utilities.formatDate(new Date(), Session.getScriptTimeZone(), "yyyy-MM-dd");

            // Create the email subject with the current date
            var subject = "Converted PDF - " + currentDate;

            // Add the robot emoji to the email body
            var body = "Please find the attached PDF.";

            // Define the recipients
            var recipient1 = "mailinglist@example.com";
            var recipient2 = "secondemail@example.com";
            var recipients = recipient1 + "," + recipient2;

            // Send email with PDF attachment to multiple recipients
            GmailApp.sendEmail(recipients, subject, body, {
              attachments: [pdfBlob]
            });

            // Clean up: Remove files from Drive
            DriveApp.getFileById(convertedFile.id).setTrashed(true);
            file.setTrashed(true);

          } catch (e) {
            Logger.log('Error processing attachment: ' + e.toString());
          }
        }
      }
    }
    // Remove label to mark as processed
    threads[i].removeLabel(label);
  }
}
```
2.5: Customize the Script

	•	Update Email Addresses:
	•	Replace "mailinglist@example.com" and "secondemail@example.com" with your actual email addresses.
```
var recipient1 = "yourfirstemail@example.com";
var recipient2 = "yoursecondemail@example.com";
```


	•	Modify Email Content (Optional):
	•	Customize the subject and body variables as desired.

Step 3: Authorize and Test the Script

	1.	Authorize the Script:
	•	Click on the “Save project” icon (💾).
	•	Select the processODTEmails function from the dropdown menu.
	•	Click the Run button (▶️).
	•	Follow the prompts to authorize the script:
	•	Select your Google account.
	•	Click “Advanced” and then “Go to [Project Name] (unsafe)” if prompted.
	•	Review permissions and click “Allow”.
	2.	Test the Script:
	•	Send a test email to your Gmail account with an ODT attachment.
	•	Ensure it gets labeled as ODT_Processing.
	•	Run the script manually by clicking the Run button.
	•	Check that the PDF version is sent to the specified email addresses.
	•	Verify that the original email label is removed or the email is archived.

Step 4: Set Up Triggers for Automation

Automate the script to run at regular intervals.

	1.	Open Triggers:
	•	In the Apps Script editor, click on the Triggers icon (⏰) on the left sidebar.
	•	Alternatively, go to “Edit” > “Current project’s triggers”.
	2.	Create a New Trigger:
	•	Click on “Add Trigger” (➕).
	3.	Configure the Trigger:
	•	Choose which function to run: processODTEmails
	•	Deployment: Head
	•	Select event source: Time-driven
	•	Select type of time-based trigger: Minutes timer
	•	Select minute interval: Every 5 minutes (or your preferred interval)
	•	Failure notification settings: Notify me immediately on failure
	•	Click “Save”.
	4.	Verify the Trigger:
	•	The trigger should now appear in the list of project triggers.

Additional Considerations

Error Handling

	•	Logging:
	•	Use Logger.log() to log information or errors.
	•	View logs by clicking “View” > “Logs” in the Apps Script editor.
	•	Notifications:
	•	Modify the script to send an email to yourself if an error occurs.

 ```
 catch (e) {
  Logger.log('Error processing attachment: ' + e.toString());
  GmailApp.sendEmail("youremail@example.com", "Script Error", e.toString());
}
```


Quotas and Limits

	•	Gmail Sending Limits:
	•	Standard Gmail accounts have a daily sending limit of 500 emails.
	•	Google Workspace accounts have higher limits.
	•	Apps Script Quotas:
	•	Be aware of execution time limits and other quotas.
	•	Reference: Apps Script Quotas

Security Best Practices

	•	Protect Sensitive Data:
	•	Do not share your script or project with untrusted parties.
	•	Review Permissions:
	•	The script requires access to Gmail and Drive. Ensure you’re comfortable with these permissions.
	•	Monitor Activity:
	•	Regularly check your sent emails and script executions for any anomalies.

Conclusion

By following this guide, you’ve set up an automated system to convert ODT files received via email into PDFs and forward them to specified recipients. This automation leverages Google’s free tools and services, eliminating the need for additional hosting costs.
