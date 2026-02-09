# n8n ticket excel automation

This is a project that automates entries of tickets into excel that come in through a ticketing system.

# Jira
First step is to get a Jira API token through this link: https://id.atlassian.com/manage-profile/security/api-tokens.  
Then click on Create API token and name it n8n-ticket-export and create and copy the token.  

# n8n
If you have a docker container for n8n and saved it somewhere, open up the link and go to the homepage to create a n8n workflow.  
Then when in the new n8n workflow, for the trigger search for the Schedule Trigger option and you will see this:  
