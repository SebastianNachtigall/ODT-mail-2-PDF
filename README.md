# ODT-mail-2-PDF
A little Google Script that scans your inbox for a specific mail (with a label) and a ODT attachment. It then converts the ODT file to a PDF and sends it out as another email.



 # What does this script?
 •	Monitors emails with the “ODT_Processing” label.
	•	Downloads the ODT attachment to Google Drive.
	•	Converts the ODT file to a Google Doc to ensure compatibility.
	•	Exports the Google Doc as a PDF file.
	•	Sends an email with the PDF attached to your mailing list.
	•	Moves the processed email to a different label or archives it to prevent reprocessing.

# Setting up your Gmail account and preparing the Script

1.	Set Up a Gmail Filter:
	•	Create a filter in Gmail that labels incoming emails with ODT attachments.
	•	Go to Gmail settings > Filters and Blocked Addresses > Create a new filter.
	•	Set criteria to filter emails that have attachments and possibly from specific senders.
	•	Apply a unique label like “ODT_Processing”.

2.	Insert the Google Apps Script from this repository:
	•	Open Google Apps Script and create a new project.
	•	copy & paste the Script from this repo.
    Note: Replace "mailinglist@example.com" with your actual mailing list address.
	•	You’ll need to enable the Drive API in your Apps Script project:
    • In the script editor, go to Resources > Advanced Google Services.
	  •	Turn on Drive API. I use v2 
  •	Then, go to Google Cloud Platform API Console and enable the Drive API for your project.
  	• In the “Advanced Google Services” dialog (from the previous step), you’ll see a link at the bottom that says “Google Cloud Platform API dashboard”. Click on it.
	  •	Alternatively, go directly to the Google Cloud Console.
	  •	Ensure you’re in the correct project. The project name is usually in the format “My Project” followed by a number.
	  •	Enable the Drive API:
	  •	In the left sidebar, click on “APIs & Services” > “Library”.
	  •	In the search bar, type “Drive” and press Enter.
	  •	Click on “Google Drive API” from the search results.
	  •	Click the “Enable” button.
      After enabling the Drive API, you might see a banner at the top of the page that says something like:
      “To use this API, you may need credentials. Click ‘Create credentials’ to get started.”
      Do you need to create credentials?
	    •	For Apps Script projects that you run yourself, you generally do not need to create additional credentials.
	    •	Apps Script handles the authorization process internally, and when you run your script, it will prompt you to authorize the necessary permissions.

	3.	Set Up Triggers:
	•	Set up a time-driven trigger to run the script every few minutes.
	•	In the script editor, click on the clock icon (Triggers).
	•	Add a new trigger for processODTEmails function.
	•	Choose “Time-driven” and set the interval (e.g., every 5 minutes).

 4.	Test the Setup:
	•	Send an email with an ODT attachment to your Gmail.
	•	Ensure it gets labeled correctly.
	•	Wait for the script to run and check if the PDF is sent to the mailing list.

 5.	Monitor and Adjust:
	•	Check the execution logs in Apps Script for any errors.
	•	Adjust the script if necessary to handle edge cases.

