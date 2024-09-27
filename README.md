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
