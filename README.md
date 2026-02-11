# n8n ticket excel automation

This is a project that automates entries of tickets into excel that come in through a ticketing system. This script takes the tedious manual task of inputting new tickets into Google Sheets to accuractly record new tickets that are entered in within the system.  

# Jira
First step is to get a Jira API token through this link: https://id.atlassian.com/manage-profile/security/api-tokens.  
Then click on Create API token and name it n8n-ticket-export and create and copy the token.  

# n8n
If you have a docker container for n8n and saved it somewhere, open up the link and go to the homepage to create a n8n workflow.  
Then when in the new n8n workflow, for the trigger search for the Schedule Trigger option and you will see this:  
<img width="1875" height="857" alt="image" src="https://github.com/user-attachments/assets/68dd6c49-ad21-4f0b-9d3b-14abeb20aff8" />  
Then add the Jira node, choose get many issues and you will see this:  
<img width="1870" height="868" alt="image" src="https://github.com/user-attachments/assets/eacb5e23-948d-4f30-a724-2e2927e94bcc" />  
In the credentials, make sure to have the API token saved, along with the email you used for Jira, and the website that you might have made. Then save it and add this JQL function like below:  
<img width="416" height="151" alt="image" src="https://github.com/user-attachments/assets/580b2c1d-cecd-4370-a620-9b410d308810" />  
Then add the Edit Nodes section and see this:  
<img width="1864" height="856" alt="image" src="https://github.com/user-attachments/assets/87a69da2-3057-4640-9bd3-747388fd1fad" />  
Add in the relevant fields that will be in the Excel file:  
<img width="411" height="674" alt="image" src="https://github.com/user-attachments/assets/1d50b054-748e-4a2f-b246-23ba8d801e71" />  
<img width="401" height="701" alt="image" src="https://github.com/user-attachments/assets/42104390-78c2-4949-bb21-eb4c3452eb16" />  
Then add the node for Google Sheets like below:  
<img width="468" height="856" alt="image" src="https://github.com/user-attachments/assets/c790f1fe-1fa6-469d-9990-90fbfbfc16e6" />  
<img width="423" height="602" alt="image" src="https://github.com/user-attachments/assets/162efcc4-22ed-4fc7-8a64-14f46fd732da" />  
<img width="412" height="532" alt="image" src="https://github.com/user-attachments/assets/6ea635ea-68e1-4f7c-8559-af1fadec19d9" />  


You will need to set up credential access for Google Cloud first. Go to projects on Google cloud and create a new project and create it.  
Then go to APIs & Services and click on Library, then search for Google Sheets API and enable it.  
Then go to APIs & Services -> OAuth consent screen, choose external, click create then add required fields such as app name, user support email and dev contact, hit save and continue.  
Then add a test user account by going to verification center. Then go back to APIs and Services -> Credentials, clcik on create credentials -> OAuth Client ID, web applications, application type as web application, and a URL as the http://localhost:5678/rest/oauth2-credential/callback, then click create.  
Then copy the client ID and client secret IDs and copy and paste them into n8n for the Google Sheets info. Also don't forget to enable the Google Drive API as well for email sign in to work.  

# n8n Email report

To make this standout, lets add another workflow:  
<img width="1181" height="255" alt="image" src="https://github.com/user-attachments/assets/4db676c0-74f1-4dce-be89-df10fb0f243a" />  
The Daily Email Trigger is the same as the first node, except set to run every day ata 8 am in the morning, but for this will be tested every minute or so which leads into ->  
Get Daily Tickets: Same as before but with a different JQL:  
<img width="422" height="853" alt="image" src="https://github.com/user-attachments/assets/4626079e-e4d2-4b95-a39e-7b0ff3cafae8" />  
This JQL checks for created tickets at the start od the day and orders by most recently created. Which feeds into the code by JavaScript part ->  
*Generated the code using Claude*  
<img width="859" height="532" alt="image" src="https://github.com/user-attachments/assets/3e5e3fc8-386a-4fc8-bbf9-2a49a9c0e825" />  
<img width="827" height="373" alt="image" src="https://github.com/user-attachments/assets/d4f7d91b-6875-4cce-b0af-ebfa7b197055" />  
<img width="848" height="351" alt="image" src="https://github.com/user-attachments/assets/3ceaadfe-e132-4f7b-8815-e0677c113cab" />  
This code ensures a good setup of the daily email notifier which feeds into the -> Sending a email over Gmail.  
<img width="421" height="807" alt="image" src="https://github.com/user-attachments/assets/474345c9-fe9c-448e-9f81-9ca51778b0c0" />  
With the end result like this:  
<img width="1489" height="266" alt="image" src="https://github.com/user-attachments/assets/18b3c3e3-d51f-41c1-867b-8a0695920ca7" />  


# Setup  
<img width="1255" height="378" alt="image" src="https://github.com/user-attachments/assets/e1c281b8-08de-440f-b957-f32bc6878f8a" />  
This is the setup for the workflow.  
Schedule Trigger checks for a new ticket every five minutes which leads into ->  
Get Many Issues for Jira: Takes an API token from the Personal account, checks for new tickets every 15 minutes, and sends it on over to ->  
Edit Fields: Takes the Ticket data in json format which leads to ->  
Append or Update Row in Sheets -> Relies on Google Cloud APIs for Drive and Sheets to be on and takes the json data and inputs them into the Sheets document to record the data.  

For the second workflow:  
<img width="1202" height="439" alt="image" src="https://github.com/user-attachments/assets/ba37a683-fcd3-4e2a-8dbe-72feb0295a51" />  
This is the second setup for it.  
Daily Email Trigger checks for new tickets in the system at 8 am every morning which leads into ->  
Get Daily Tickets: Takes an API token from the Personal account, checks for new tickets and their summary, and sends it on over to ->  
Code in JavaScript: Formats the email format which leads into ->  
Send a Message: Sends a gmail message with the tickets that were created in that 24HR period.  


# Testing

# Live









