Complete Setup Guide
This comprehensive guide will walk you through setting up the N8N Social Media AI Automation workflow from scratch.
Table of Contents

Prerequisites
n8n Installation
API Credentials Setup
Google Sheets Configuration
Workflow Import
Configuration
Testing
Troubleshooting


Prerequisites
Required Accounts

 n8n instance (cloud or self-hosted)
 Google Account (for Sheets)
 OpenAI Account (with API access)
 Facebook Page (for Facebook/Instagram)
 Twitter/X Account
 Pinterest Business Account
 LinkedIn Account/Page

Technical Requirements

Basic understanding of APIs
Access to create OAuth apps
Ability to configure webhooks


n8n Installation
Option 1: n8n Cloud (Easiest)

Visit n8n.cloud
Sign up for an account
Choose your plan
Access your n8n instance

Option 2: Self-Hosted (Docker)
bash# Pull the latest n8n image
docker pull n8nio/n8n

# Run n8n
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
Option 3: Self-Hosted (npm)
bash# Install n8n globally
npm install n8n -g

# Start n8n
n8n start
Access n8n: Open your browser to http://localhost:5678

API Credentials Setup
1. OpenAI API Key
Steps:

Go to OpenAI Platform
Navigate to API Keys
Click "Create new secret key"
Copy and save the key securely

In n8n:

Go to Credentials > Add Credential
Search for "OpenAI"
Select "OpenAI API"
Paste your API key
Name it (e.g., "OpenAI account")
Save

Cost Consideration: GPT-5 API calls will incur costs. Monitor your usage at OpenAI dashboard.

2. Google Sheets OAuth2
Steps:

Go to Google Cloud Console
Create a new project or select existing
Enable Google Sheets API
Go to Credentials > Create Credentials > OAuth client ID
Configure consent screen:

User Type: External
Add your email as test user


Create OAuth 2.0 Client ID:

Application type: Web application
Authorized redirect URIs: https://YOUR-N8N-URL/rest/oauth2-credential/callback


Copy Client ID and Client Secret

In n8n:

Credentials > Add Credential
Search "Google Sheets"
Select "Google Sheets OAuth2 API"
Enter Client ID and Client Secret
Click "Connect my account"
Authorize in popup
Save as "Google Sheets account"


3. Facebook/Instagram (Graph API)
Steps:

Go to Facebook Developers
Create an App:

Type: Business
Use case: Other


Add Facebook Login product
Add Instagram Graph API product
Go to Settings > Basic

Copy App ID and App Secret


Generate Access Token:

Go to Tools > Graph API Explorer
Select your Page
Add permissions:

pages_manage_posts
pages_read_engagement
instagram_basic
instagram_content_publish


Generate Token


Convert to Long-Lived Token:

   https://graph.facebook.com/oauth/access_token?
   grant_type=fb_exchange_token&
   client_id=YOUR_APP_ID&
   client_secret=YOUR_APP_SECRET&
   fb_exchange_token=YOUR_SHORT_TOKEN
Get Page Access Token:
https://graph.facebook.com/me/accounts?access_token=YOUR_LONG_LIVED_TOKEN
Get Instagram Business Account ID:
https://graph.facebook.com/v19.0/me/accounts?fields=instagram_business_account&access_token=YOUR_PAGE_TOKEN
In n8n:

Credentials > Add Credential
"Facebook Graph API"
Enter Access Token
Name it "Facebook Page Posting"
Repeat for Instagram with Instagram token


4. Twitter/X API
Steps:

Go to Twitter Developer Portal
Apply for Developer Account
Create a Project and App
Generate OAuth 2.0 credentials:

Go to App Settings
User authentication settings
Set up OAuth 2.0
Type: Web App
Callback URL: https://YOUR-N8N-URL/rest/oauth2-credential/callback
Permissions: Read and write


Copy Client ID and Client Secret

In n8n:

Credentials > Add Credential
"Twitter OAuth2 API"
Enter Client ID and Client Secret
Click "Connect my account"
Authorize
Save as "X Astoria" (or your name)


5. Pinterest API
Steps:

Go to Pinterest Developers
Create an App
Get access token:

Go to App settings
Generate Access Token
Scopes needed:

boards:read
boards:write
pins:read
pins:write




Copy the token

In n8n:

Credentials > Add Credential
"HTTP Bearer Auth"
Enter your token
Name it "Pinterest Astoria"

Note: Workflow uses sandbox API. For production, update URLs to remove -sandbox.

6. LinkedIn API
Steps:

Go to LinkedIn Developers
Create an App
Add Products:

Share on LinkedIn
Sign In with LinkedIn


OAuth 2.0 settings:

Redirect URL: https://YOUR-N8N-URL/rest/oauth2-credential/callback


Copy Client ID and Client Secret
Get your Person URN:

   https://api.linkedin.com/v2/me
In n8n:

Credentials > Add Credential
"LinkedIn OAuth2 API"
Enter Client ID and Client Secret
Connect account
Save


Google Sheets Configuration
Create Your Content Sheet
Template Structure:
Column NameDescriptionExampleTitlePost title"New Product Launch"Short DescriptionBrief description"Introducing our latest innovation"Main KeywordsSEO keywords"innovation, tech, product"Image URLPublic image URL"https://example.com/image.jpg"Post URLLanding page URL"https://yoursite.com/product"StatusWorkflow status"New" / "Pending" / "Posted"Pinterest BoardBoard name"Products"Facebook CaptionAI-generated(auto-filled)Facebook TagsAI-generated hashtags(auto-filled)Facebook StatusPost status(auto-filled)Instagram CaptionAI-generated(auto-filled)Instagram TagsAI-generated hashtags(auto-filled)Instagram StatusPost status(auto-filled)Twitter CaptionAI-generated(auto-filled)Twitter TagsAI-generated hashtags(auto-filled)Twitter StatusPost status(auto-filled)Pinterest TitleAI-generated(auto-filled)Pinterest DescriptionAI-generated(auto-filled)Pinterest TagsAI-generated hashtags(auto-filled)Pinterest StatusPost status(auto-filled)LinkedIn CaptionAI-generated(auto-filled)LinkedIn TagsAI-generated hashtags(auto-filled)LinkedIn StatusPost status(auto-filled)Image PromptAI image prompt(auto-filled)
Setup Steps:

Create New Sheet:

Open Google Sheets
Create a new spreadsheet
Name it "Social Media Posts" or similar


Add Headers:

Copy all column names from the table above
Paste in row 1


Share Sheet:

Click "Share"
Copy the sharing link
Extract the Sheet ID from URL:



     https://docs.google.com/spreadsheets/d/SHEET_ID_HERE/edit

Sheet Name:

Rename Sheet1 to "Social Posts"
Or note your sheet name




Workflow Import
Import the Workflow

Download workflow.json from this repository
Open n8n
Click Workflows > Import from File
Select the downloaded JSON file
Click Import

Verify Import
You should see:

~40+ nodes organized in sections
Sticky notes with section labels
Connected workflow paths


Configuration
Update All Credential References
Google Sheets Nodes:

Find all "Google Sheets" nodes (8 total)
For each node:

Click on the node
Select your "Google Sheets account" credential
Update Document ID (your Sheet ID)
Update Sheet Name ("Social Posts")



OpenAI Node:

Find "OpenAI Chat Model" node
Select your "OpenAI account" credential
Verify model is "gpt-5" or update to "gpt-4" if needed

Facebook/Instagram Nodes:

"Facebook Graph API" node:

Update credential
Update node path: /YOUR_PAGE_ID/photos


"Facebook Graph API1" and "Facebook Graph API2":

Update credential
Update node path: /YOUR_INSTAGRAM_ID/media



Twitter Node:

"Create Tweet1" node
Update credential

Pinterest Nodes:

All Pinterest HTTP Request nodes
Update Bearer token credential
Update API URLs (remove -sandbox for production)

LinkedIn Nodes:

"Create a post" node
Update credential
Update person ID: YOUR_PERSON_URN

Platform-Specific IDs
Find Your IDs:
Facebook Page ID:
https://graph.facebook.com/me/accounts?access_token=YOUR_TOKEN
Instagram Business Account ID:
https://graph.facebook.com/YOUR_PAGE_ID?fields=instagram_business_account&access_token=YOUR_TOKEN
LinkedIn Person URN:
https://api.linkedin.com/v2/me
Look for the id field.

Testing
Test Content Generation (Part 1)

Add Test Data:

Open your Google Sheet
Add a test row:



     Title: Test Post
     Short Description: This is a test
     Main Keywords: test, automation, n8n
     Image URL: https://via.placeholder.com/1200x630
     Post URL: https://example.com
     Status: New
     Pinterest Board: Test Board

Trigger Workflow:

Click "Chat" node
Send any message
Watch workflow execute


Verify:

Check Google Sheet
Status should change to "Pending"
All caption fields should be filled
Review generated content quality



Test Publishing (Part 2)

Manual Trigger:

Click "When clicking 'Execute workflow'" node
Click "Test workflow"


Monitor Progress:

Watch each platform section
Check for errors
Verify status updates


Verify Posts:

Check each platform
Confirm posts appeared
Verify images loaded correctly



Image Format Testing
Your images should have these variants:

Original: image.jpg
Landscape: image-landscape.jpg (Facebook, LinkedIn, Twitter)
Portrait: image-portrait.jpg (Instagram, Pinterest)


Troubleshooting
Common Issues
1. "Credential not found"
Solution:

Verify all credentials are created
Update credential IDs in all nodes
Re-authenticate if needed

2. "Google Sheets row not found"
Solution:

Check Status column is exactly "New" or "Pending"
Verify Sheet ID is correct
Ensure sheet name matches

3. "Facebook Graph API error"
Solution:

Verify token hasn't expired
Check permissions are correct
Confirm Page ID is correct
Ensure page is published

4. "Image format error"
Solution:

Verify image URLs are publicly accessible
Check image format variants exist
Test URL directly in browser

5. "Rate limit exceeded"
Solution:

Wait nodes are set to 3 seconds
Increase if hitting limits
Check platform-specific limits

6. "OpenAI API error"
Solution:

Verify API key is valid
Check account has credits
Ensure model name is correct

Error Codes
Facebook:

(#100) - Invalid parameters
(#190) - Token expired
(#368) - Temporarily blocked

Twitter:

429 - Rate limit
401 - Authentication failed
403 - Forbidden

Pinterest:

401 - Invalid token
429 - Rate limit
404 - Board not found

Debug Tips

Enable Workflow Debugging:

Click workflow settings
Enable "Save execution data"
Review failed executions


Test Individual Nodes:

Click "Execute node"
Check output data
Verify API responses


Check Logs:

View execution history
Look for error messages
Check timing issues




Optimization Tips
Performance

Adjust wait times based on your rate limits
Use batch processing for multiple posts
Schedule during off-peak hours

Content Quality

Refine AI prompts for your brand voice
Add custom instructions per platform
Test different keyword combinations

Reliability

Set up error notifications
Monitor API quotas
Keep credentials updated


Next Steps
After successful setup:

✅ Test with real content
✅ Adjust AI prompts to your style
✅ Set up scheduled execution
✅ Monitor performance metrics
✅ Scale to more content


Support
If you encounter issues:

Check this guide thoroughly
Review n8n documentation
Check platform API documentation
Open an issue on GitHub


Congratulations! 🎉 Your AI-powered social media automation is ready to use!