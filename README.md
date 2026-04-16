Building a Podcast Generator
What We're Building
Frontend : Create a beautiful interface first
N8n Workflow: Update our workflow to accept requests
Connection: Link them seamlessly together
Steps to Follow
Creating a Beautiful Frontend

Open Lovable Platform
Go to lovable.dev
Login to the website
Paste your prompt
Test the Interface
Type some sample text
Click the Generate Podcast button
See the loading animation
Connecting the Frontend to n8n Workflow

Providing Data to n8n
To generate a podcast, n8n needs information We'll send this data in a specific format.
{"text": "whatever topic they typed"}

The Missing Link
We need to create a bridge between:

The beautiful interface (what people see) and n8n podcast generator workflow (what creates the podcast)
Webhooks
A webhook serves as a bridge between the frontend application and n8n workflow by automatically receiving incoming data
WebHook Node
A webhook node act as a trigger node for a workflow when you want to receive data and run a workflow based on the data
It is essentially a URL endpoint that listens for incoming data
Webhooks - The Bridge Builder!

Think of a webhook like giving your workflow a phone number on the internet:

Your frontend knows this phone number
It "calls" this number with the user's text - whenever a user enters text and presses the button
Your workflow "answers" - processes the input and generates the podcast
It "calls back" with the finished audio file - returns the final result
Replace Chat Trigger with Webhook
Why this change?
Chat Trigger only works inside n8n
Webhook works from anywhere on the internet
Steps to Update:
Open your existing Podcast Generator workflow in n8n
Delete the Chat Trigger node
Add a Webhook node as the starting point
Configure the Webhook node settings
Configuring the Webhook Node:
Open the Webhook node settings
Set HTTP Method to POST
Set Response Mode to Respond to Webhook
Copy the generated webhook URL (looks like: https://your-n8n.app/webhook/abc123xyz)
How Webhooks Work
The frontend sends data to the webhook URL, and the webhook passes it to your workflow for processing
Updating Data Connections
Connect Webhook to basic LLM chain node
In LLM chain settings :
In the Source for Prompt (User Message), change dropdown to Define below
Update the input value to: {{ $json.body.text }}
Similarly, update the System prompt placeholder to: {{ $json.body.text }}
This captures the text that comes from your frontend
Keep all other connections the same
Updating Data Connections
Complete Workflow Flow Webhook (Receives text) → LLM Chain (Processes text) → Gemini → Murf.ai
Current Situation
Right now, the workflow:

Receives data from frontend
Processes the request
Generates podcast audio
But the audio is stuck inside n8n!
The frontend is waiting for a response, but none has been sent yet.

Send Data Back – Respond to Webhook Node
A node that completes the webhook conversation
Takes data from your workflow
Sends it back to whoever called the webhook
Set up the Response Node
Add Respond to Webhook node after your HTTP Response/Podcast Downloader node
In the node settings, configure:
Respond With: Binary File (because our output is audio)
Response Data Source: Automatically from Input
This ensures the workflow sends the generated audio back as a downloadable file or playable URL to the frontend.
Connecting Everything Together

Specifying Frontend Integration Details
Previously, we created a basic frontend without specifying:
Which webhook URL to call?
What data format to expect?
How to handle the response?
Connecting the Frontend to n8n
Now we will update our frontend with the actual connection details from our n8n workflow configuration
Update your Frontend
Go back to your Lovable project
Copy your webhook URL from n8n
Paste the connection prompt, replacing [PASTE YOUR WEBHOOK URL HERE] with your actual webhook URL
Wait for Lovable to update the interface

What Happens Behind the Scenes

The Complete Flow

1. Frontend
Packages the topic as JSON
Sends data to n8n webhook
2. n8n Webhook
Receives the JSON data
Triggers the workflow
3. Audio File URL
Workflow processes and generates audio
Returns audio file URL
4. Podcast Player
Waits for the audio file URL
Shows the podcast player when ready
User can play the generated podcast
Test the Complete System
Step-by-Step Testing
Execute the workflow in n8n (to keep it ready)
Open your Lovable preview
Type a podcast topic (e.g., "Generative AI")
Click the Generate Podcast button
Watch the loading animation appear
Switch to the n8n tab — see nodes turning green as workflow executes
Wait 10-30 seconds for processing
Return to Lovable — audio player appears with your podcast
Click Play and listen to your AI-generated podcast!
Workflow Should be in Active State
To ensure your workflow is automated and triggers as expected, make sure the workflow is active

Open your workflow in n8n
Toggle the workflow status to Active
The workflow will now automatically respond to incoming webhook requests

Note
The Active button has been updated to Publish, allowing you to publish your workflows.
Kindly note that the Publish feature is restricted to a limited number of uses on the portal.
Deploy our Application
Right now, your app works in Lovable's preview Deployment gives it a real home on the internet where anyone can visit!

Deployment
In Lovable, click the Publish button (top right corner)
Wait 1-2 minutes while Lovable deploys your project
Receive your public URL (format: https://yourapp.lovable.app)
Share the URL with friends and family!

Before vs After

BEFORE:
Workflow hidden inside n8n
Only works with Chat Trigger
Can't share with others
Manual execution required

AFTER:
Professional web application
Beautiful public interface
Anyone can use it
Your own podcast generation service!
